// Minimal safe menu + wrapper. Paste into Apps Script, Save, then reload the spreadsheet UI.
function onOpen() {
  SpreadsheetApp.getUi()
    .createMenu('GHM Autos')
    .addItem('Refresh Active Tab', 'refreshActiveTab')
    .addSeparator()
    .addItem('Refresh All (Sandbox)', 'runRefreshAutosSandbox')
    .addToUi();
}

function refreshActiveTab() {
  const ss = SpreadsheetApp.getActive();
  const sheetName = ss.getActiveSheet().getName();
  SpreadsheetApp.getUi().alert('Running Refresh on active tab: ' + sheetName);
  // Prefer the explicit wrapper if it exists, otherwise call RUN_updateAll_now() if present.
  try {
    if (typeof runRefreshAutosForSheet === 'function') {
      runRefreshAutosForSheet(sheetName);
      return;
    }
  } catch(e){ /* ignore */ }
  try {
    if (typeof runRefreshAutosSandbox === 'function') {
      // best-effort: run the sandbox full-run if no per-sheet wrapper exists
      runRefreshAutosSandbox();
      return;
    }
  } catch(e){ /* ignore */ }
  try {
    if (typeof RUN_updateAll_now === 'function') {
      RUN_updateAll_now();
      return;
    }
  } catch(e){ /* ignore */ }
  SpreadsheetApp.getUi().alert('No runner function found (runRefreshAutosForSheet / runRefreshAutosSandbox / RUN_updateAll_now). Please restore the pipeline functions.');
}

