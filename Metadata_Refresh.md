/**
 * refreshDerivedMetadata(sheetName)
 * - Recomputes meta_title_auto, print_eligible_inferred, alt_text_en_auto,
 *   hook_line, attributes_json, and ensures media_filename has an extension.
 * - Safe: creates a sheet backup and writes QA_Report_Derived.
 *
 * Usage:
 *  - paste into Apps Script, Save, then Run refreshDerivedMetadata() or refreshDerivedMetadata('Manifest_GHM_Olympians')
 */

function refreshDerivedMetadata(sheetName) {
  const ss = SpreadsheetApp.getActive();
  const sh = sheetName ? ss.getSheetByName(sheetName) : ss.getActiveSheet();
  if (!sh) throw new Error('Sheet not found: ' + (sheetName||'(active)'));

  // backup single sheet
  const now = Utilities.formatDate(new Date(), ss.getSpreadsheetTimeZone(), 'yyyyMMdd_HHmm');
  sh.copyTo(ss).setName(sh.getName() + '_Backup_Derived_' + now);

  const headers = sh.getRange(1,1,1,sh.getLastColumn()).getValues()[0];
  const idx = h => headers.indexOf(h);

  const H = {
    Series: idx('Series'),
    Character: idx('Character'),
    Variant: idx('Character_Variant'),
    myth_scene: idx('myth_scene'),
    Style: idx('Style'),
    Colorway: idx('Colorway') >= 0 ? idx('Colorway') : (idx('Colourway') >= 0 ? idx('Colourway') : -1),
    Frame: idx('Frame'),
    Tier: idx('Tier'),
    edition_size: idx('edition_size'),
    title_display: idx('title_display'),
    title_en: idx('title_en'),
    meta_title_auto: idx('meta_title_auto'),
    print_eligible_inferred: idx('print_eligible_inferred'),
    alt_text_en_auto: idx('alt_text_en_auto'),
    hook_line: idx('hook_line'),
    attributes_json: idx('attributes_json'),
    media_filename: idx('media_filename'),
    image_mime: idx('image_mime'),
    image_url: idx('image_url'),
    image_filename: idx('image_filename'),
    name_final: idx('name_final'),
    slug: idx('slug')
  };

  // Ensure target columns exist (append if missing)
  function ensureCol(name) {
    let i = headers.indexOf(name);
    if (i === -1) {
      sh.getRange(1, headers.length + 1).setValue(name);
      headers.push(name);
      i = headers.length - 1;
    }
    return i;
  }
  H.meta_title_auto = H.meta_title_auto >=0 ? H.meta_title_auto : ensureCol('meta_title_auto');
  H.print_eligible_inferred = H.print_eligible_inferred >=0 ? H.print_eligible_inferred : ensureCol('print_eligible_inferred');
  H.alt_text_en_auto = H.alt_text_en_auto >=0 ? H.alt_text_en_auto : ensureCol('alt_text_en_auto');
  H.hook_line = H.hook_line >=0 ? H.hook_line : ensureCol('hook_line');
  H.attributes_json = H.attributes_json >=0 ? H.attributes_json : ensureCol('attributes_json');
  H.media_filename = H.media_filename >=0 ? H.media_filename : ensureCol('media_filename');

  const lastRow = sh.getLastRow();
  if (lastRow < 2) { SpreadsheetApp.getUi().alert('No data rows'); return; }
  const data = sh.getRange(2,1,lastRow-1,sh.getLastColumn()).getValues();

  const report = [];

  // helpers
  function clean(s){ return (s===null||s===undefined) ? '' : String(s).replace(/\s+/g,' ').trim(); }
  function cleanColorway(s){
    if (!s) return '';
    s = clean(s);
    s = s.replace(/palette/i,'').trim(); // remove redundant 'palette' word when present
    // correct common typos
    s = s.replace(/\bPallette\b/i,'Palette');
    return s;
  }
  function mimeToExt(mime){
    if(!mime) return '';
    mime = mime.split(';')[0].trim().toLowerCase();
    const map = {'image/png':'png','image/jpeg':'jpg','image/jpg':'jpg','image/gif':'gif','image/webp':'webp','image/svg+xml':'svg','video/mp4':'mp4','video/webm':'webm'};
    return map[mime] || '';
  }
  function extFromUrl(url){
    try {
      if(!url) return '';
      const p = url.split('?')[0];
      const m = p.match(/\.([a-z0-9]{1,6})$/i);
      return m ? m[1].toLowerCase() : '';
    } catch(e){ return ''; }
  }
  function toJSONAttributes(rowObj){
    const arr = [];
    const push = (t,v) => { if (v!=='' && v!==null && v!==undefined) arr.push({trait_type: t, value: String(v)}); };
    push('Pantheon', rowObj.Pantheon || rowObj.pantheon || '');
    push('Series', rowObj.Series || '');
    push('Character', rowObj.Character || '');
    push('Variant/Location', rowObj.Character_Variant || '');
    push('Myth Scene', rowObj.myth_scene || '');
    push('Style/Format', rowObj.Style || '');
    push('Tier', rowObj.Tier || '');
    push('Edition Size', rowObj.edition_size || '');
    push('Colorway', rowObj.Colorway || '');
    push('Frame', rowObj.Frame || '');
    return JSON.stringify(arr);
  }

  for (let r=0; r<data.length; r++){
    const rowNum = r + 2;
    const row = data[r];

    const Series = clean(H.Series>=0 ? row[H.Series] : '');
    const Character = clean(H.Character>=0 ? row[H.Character] : '') || clean(H.name_final>=0 ? row[H.name_final] : '');
    const Variant = clean(H.Variant>=0 ? row[H.Variant] : '');
    const myth = clean(H.myth_scene>=0 ? row[H.myth_scene] : '');
    const Style = clean(H.Style>=0 ? row[H.Style] : '');
    const ColorwayRaw = H.Colorway>=0 ? row[H.Colorway] : '';
    const Colorway = cleanColorway(ColorwayRaw);
    const Frame = clean(H.Frame>=0 ? row[H.Frame] : '');
    const Tier = clean(H.Tier>=0 ? row[H.Tier] : '');
    const edition_size = clean(H.edition_size>=0 ? row[H.edition_size] : '');
    const title_en = clean(H.title_en>=0 ? row[H.title_en] : '');
    const slug = clean(H.slug>=0 ? row[H.slug] : '');
    const image_mime = clean(H.image_mime>=0 ? row[H.image_mime] : '');
    const image_url = clean(H.image_url>=0 ? row[H.image_url] : '');
    let media_fn = clean(H.media_filename>=0 ? row[H.media_filename] : (H.image_filename>=0 ? row[H.image_filename] : ''));

    // 1) meta_title_auto: prefer title_en; fallback to title_display; append Series
    const titleBase = title_en || clean(H.title_display>=0 ? row[H.title_display] : '');
    let metaTitle = titleBase;
    if (Series) metaTitle = metaTitle ? (metaTitle + ' | ' + Series) : Series;
    // optional short site tagline could be appended if you want; leaving it minimal
    // write if changed
    const oldMeta = clean(row[H.meta_title_auto]);
    if (metaTitle && oldMeta !== metaTitle) {
      sh.getRange(rowNum, H.meta_title_auto + 1).setValue(metaTitle);
      report.push([rowNum, 'meta_title_auto', oldMeta, metaTitle]);
    }

    // 2) print_eligible_inferred: YES if Tier includes "Print" or edition_size > 1
    let printEligible = (/\bprint\b/i.test(Tier) || /\bprint\b/i.test(String(edition_size)) || (Number(edition_size) > 1)) ? 'YES' : 'NO';
    // allow forcing via existing column values if desired (but we overwrite)
    const oldPrint = clean(row[H.print_eligible_inferred]);
    if (oldPrint !== printEligible) {
      sh.getRange(rowNum, H.print_eligible_inferred + 1).setValue(printEligible);
      report.push([rowNum, 'print_eligible_inferred', oldPrint, printEligible]);
    }

    // 3) alt_text_en_auto: start with title_en (clean), add myth and colorway, avoid 'palette palette'
    // Format: "<Title_en>. <Short descriptors — myth_scene, Colorway palette, Tier edition.>"
    const descParts = [];
    if (myth) descParts.push(myth);
    if (Colorway) descParts.push(Colorway + ' palette');
    let altBuilt = titleBase || Character || slug || '';
    if (descParts.length) altBuilt += '. ' + descParts.join(', ');
    if (Tier) altBuilt += '. ' + Tier + (edition_size ? ' edition.' : '.'); else altBuilt += '.';
    // clean double words and trim
    altBuilt = altBuilt.replace(/\s+palette\s+palette/i,' palette').replace(/\s{2,}/g,' ').trim();
    const oldAltAuto = clean(row[H.alt_text_en_auto]);
    if (oldAltAuto !== altBuilt) {
      sh.getRange(rowNum, H.alt_text_en_auto + 1).setValue(altBuilt);
      report.push([rowNum,'alt_text_en_auto',oldAltAuto, altBuilt]);
    }

    // 4) hook_line: "Character — Variant · Style · Tier — Myth Scene"
    let hook = Character || titleBase || '';
    if (Variant) hook = hook + ' — ' + Variant;
    const mid = [Style, Tier].filter(Boolean).join(' · ');
    if (mid) hook = hook + (hook ? ' · ' + mid : mid);
    if (myth) hook = hook + ' — ' + myth;
    const oldHook = clean(row[H.hook_line]);
    if (oldHook !== hook) {
      sh.getRange(rowNum, H.hook_line + 1).setValue(hook);
      report.push([rowNum,'hook_line',oldHook,hook]);
    }

    // 5) attributes_json: reconstruct from canonical fields and normalize Colorway spelling
    const attrsMap = {
      Pantheon: '', // optional if you have pantheon column add mapping
      Series: Series,
      Character: Character,
      'Variant/Location': Variant,
      'Myth Scene': myth,
      'Style/Format': Style,
      Tier: Tier,
      'Edition Size': edition_size,
      Colorway: Colorway,
      Frame: Frame
    };
    const attrsJSON = JSON.stringify(Object.keys(attrsMap).reduce((acc,k)=>{ if (attrsMap[k] !== '' && attrsMap[k] !== null) { acc.push({trait_type: k, value: String(attrsMap[k])}); } return acc;}, []));
    const oldAttrs = clean(row[H.attributes_json]);
    if (oldAttrs !== attrsJSON) {
      sh.getRange(rowNum, H.attributes_json + 1).setValue(attrsJSON);
      report.push([rowNum,'attributes_json', oldAttrs, attrsJSON]);
    }

    // 6) media_filename: ensure extension present; prefer existing media_filename, otherwise build from slug
    let ext = '';
    // check existing filename ext
    const fn = media_fn || (H.image_filename>=0 ? clean(row[H.image_filename]) : '');
    const hasExt = fn && /\.[a-z0-9]{1,6}$/i.test(fn);
    if (hasExt) media_fn = fn;
    else {
      // source ext: image_mime or image_url
      ext = mimeToExt(image_mime) || extFromUrl(image_url) || '';
      const base = fn || (slug || Character || titleBase).toString().toLowerCase().replace(/[^a-z0-9\-]+/g,'-').replace(/^\-+|\-+$/g,'');
      media_fn = base + (ext ? ('.' + ext) : '');
    }
    const oldMedia = clean(row[H.media_filename] || '');
    if (oldMedia !== media_fn) {
      sh.getRange(rowNum, H.media_filename + 1).setValue(media_fn);
      report.push([rowNum,'media_filename',oldMedia,media_fn]);
    }
  } // end rows loop

  // write QA report
  const RN = 'QA_Report_Derived';
  let rep = ss.getSheetByName(RN) || ss.insertSheet(RN);
  rep.clear();
  if (report.length) {
    rep.appendRow(['row','field','old','new']);
    rep.getRange(2,1,report.length,report[0].length).setValues(report);
  } else rep.appendRow(['no_changes']);
  SpreadsheetApp.getUi().alert('Derived metadata refresh complete. See ' + RN);
}

