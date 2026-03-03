/**
 * GHM — append token_id suffix to selected derived columns.
 *
 * IMPORTANT:
 * - This updater edits only the targeted columns.
 * - It does NOT rewrite the full sheet (avoids overwriting formulas in other columns).
 */

var GHM_TOKEN_SUFFIX_STRICT_COLUMNS = [
  'external_url_built',
  'json_filename',
  'edition_string',
  'media_filename'
];

var GHM_TOKEN_SUFFIX_RECOMMENDED_COLUMNS = [
  'external_url',
  'json_path',
  'json_preview_url',
  'opensea_permalink',
  'image_filename',
  'animation_filename',
  'preview_thumb'
];

/**
 * Runs only the strict columns requested.
 */
function GHM_AppendTokenIdToDerivedColumns_Active() {
  var sheet = SpreadsheetApp.getActiveSheet();
  GHM_AppendTokenIdToColumns_OnSheet_(sheet, GHM_TOKEN_SUFFIX_STRICT_COLUMNS);
}

/**
 * Runs strict columns + recommended URL/media columns.
 */
function GHM_AppendTokenIdToDerivedAndMediaColumns_Active() {
  var sheet = SpreadsheetApp.getActiveSheet();
  var cols = GHM_TOKEN_SUFFIX_STRICT_COLUMNS.concat(GHM_TOKEN_SUFFIX_RECOMMENDED_COLUMNS);
  GHM_AppendTokenIdToColumns_OnSheet_(sheet, cols);
}

/**
 * Runs strict columns on all likely collection sheets.
 */
function GHM_AppendTokenIdToDerivedColumns_AllCollections() {
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var sheets = ss.getSheets();
  for (var i = 0; i < sheets.length; i++) {
    var sh = sheets[i];
    if (!GHM_IsLikelyCollectionSheet_(sh.getName())) continue;
    GHM_AppendTokenIdToColumns_OnSheet_(sh, GHM_TOKEN_SUFFIX_STRICT_COLUMNS);
  }
}

/**
 * Runs strict + recommended columns on all likely collection sheets.
 */
function GHM_AppendTokenIdToDerivedAndMediaColumns_AllCollections() {
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var sheets = ss.getSheets();
  var cols = GHM_TOKEN_SUFFIX_STRICT_COLUMNS.concat(GHM_TOKEN_SUFFIX_RECOMMENDED_COLUMNS);

  for (var i = 0; i < sheets.length; i++) {
    var sh = sheets[i];
    if (!GHM_IsLikelyCollectionSheet_(sh.getName())) continue;
    GHM_AppendTokenIdToColumns_OnSheet_(sh, cols);
  }
}

function GHM_AppendTokenIdToColumns_OnSheet_(sheet, columnsToUpdate) {
  var lastRow = sheet.getLastRow();
  var lastCol = sheet.getLastColumn();
  if (lastRow < 2 || lastCol < 1) return;

  var header = sheet.getRange(1, 1, 1, lastCol).getValues()[0];
  var col = GHM_HeaderMap_(header);

  if (col.token_id == null) {
    throw new Error('token_id column not found on sheet: ' + sheet.getName());
  }

  var tokenRange = sheet.getRange(2, col.token_id + 1, lastRow - 1, 1);
  var tokenVals = tokenRange.getValues();

  for (var i = 0; i < columnsToUpdate.length; i++) {
    var key = String(columnsToUpdate[i] || '').toLowerCase();
    if (!key || col[key] == null) continue;

    var targetRange = sheet.getRange(2, col[key] + 1, lastRow - 1, 1);
    var currentVals = targetRange.getValues();
    var formulaVals = targetRange.getFormulas();
    var changed = false;

    for (var r = 0; r < currentVals.length; r++) {
      // Skip formula cells; keep script non-destructive.
      if (formulaVals[r][0]) continue;

      var tokenId = GHM_NormalizeTokenId_(tokenVals[r][0]);
      var current = currentVals[r][0];
      if (!tokenId || current == null || current === '') continue;

      var currentStr = String(current);
      var next = GHM_AddTokenSuffix_(currentStr, tokenId, key);
      if (next !== currentStr) {
        currentVals[r][0] = next;
        changed = true;
      }
    }

    if (changed) {
      targetRange.setValues(currentVals);
    }
  }
}

function GHM_IsLikelyCollectionSheet_(sheetName) {
  var name = String(sheetName || '').toLowerCase();
  if (!name) return false;

  // Exclude obvious non-collection/admin tabs.
  var deny = [
    'control', 'globals', 'tasks', 'overview', 'traits', 'tiers', 'planner',
    'qa_report', 'qc', 'schema', 'taxonomy', 'matrix', 'summary', 'index', 'report'
  ];
  for (var i = 0; i < deny.length; i++) {
    if (name.indexOf(deny[i]) !== -1) return false;
  }

  // Include likely collection tabs.
  var allow = ['greek', 'aztec', 'celtic', 'chinese', 'egyptian', 'hindu', 'japanese', 'roman', 'norse'];
  for (var j = 0; j < allow.length; j++) {
    if (name.indexOf(allow[j]) !== -1) return true;
  }

  // Fallback: if tab has token_id + at least one target-like column, allow by structure check later.
  return true;
}

function GHM_HeaderMap_(headerRow) {
  var map = {};
  for (var i = 0; i < headerRow.length; i++) {
    var key = String(headerRow[i] || '').trim().toLowerCase();
    if (key) map[key] = i;
  }
  return map;
}

function GHM_NormalizeTokenId_(raw) {
  if (raw == null) return '';
  var s = String(raw).trim();
  if (/^\d+\.0+$/.test(s)) s = s.replace(/\.0+$/, '');
  return s;
}

function GHM_AddTokenSuffix_(value, tokenId, columnName) {
  if (!value || !tokenId) return value;

  // If token already appears as suffix-ish token marker, keep as is.
  if (new RegExp('(?:-|_)' + GHM_EscapeRegExp_(tokenId) + '(?:$|[._/?#])').test(value)) {
    return value;
  }

  // URL-like columns.
  if (GHM_IsUrlColumn_(columnName) || /^https?:\/\//i.test(value)) {
    var parts = value.match(/^([^?#]*)(\?[^#]*)?(#.*)?$/);
    var base = parts ? parts[1] : value;
    var query = parts && parts[2] ? parts[2] : '';
    var hash = parts && parts[3] ? parts[3] : '';

    // When URL ends with slash, append token segment.
    if (/\/$/.test(base)) return base + tokenId + query + hash;

    base = base.replace(/([^/]+)$/, function(lastSeg) {
      return GHM_AddTokenSuffixToFilenameLike_(lastSeg, tokenId);
    });
    return base + query + hash;
  }

  return GHM_AddTokenSuffixToFilenameLike_(value, tokenId);
}

function GHM_IsUrlColumn_(columnName) {
  var c = String(columnName || '').toLowerCase();
  return c.indexOf('url') !== -1 || c.indexOf('permalink') !== -1 || c === 'external_url_built' || c === 'external_url';
}

function GHM_AddTokenSuffixToFilenameLike_(s, tokenId) {
  var m = s.match(/^(.*?)(\.[a-zA-Z0-9]{1,8})$/);
  if (!m) return s + '-' + tokenId;
  return m[1] + '-' + tokenId + m[2];
}

function GHM_EscapeRegExp_(s) {
  return String(s).replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
}

