# Google Sheets → Google Docs System Handover (Export-Ready Running Order)

## Purpose
This handover gives you a practical **run order** to move from active Google Sheets production work to a **complete export-ready state** for metadata, docs, and operations.

It references the existing handoff notes and script map from the latest package.

---

## 0) What exists today (baseline)
- System status: documented as previously working and ~98% complete, with active use up to Oct 2025.
- Core files in use:
  1. `GHM_Master_Manifest`
  2. `GHM_Master_Control_Sheet 1.0`
  3. `GHM_Decision_Tracker`
- Important caution: multiple scripts may be redundant/overlapping, so use a controlled sequence (below) instead of “run everything”.

---

## 1) Roles and ownership (before touching data)
Set clear owners for each checkpoint:
- **Design**: creates/updates NFT assets.
- **Admin/Data**: enters and approves NFT source data.
- **Ops/QA**: runs scripts, validates outputs, signs off export packs.
- **Publish/Claim**: uses approved exports for mint/publish; toggles claim states where needed.

> Recommended: track owners per stage in `GHM_Decision_Tracker` and mirror sign-off in Control sheet tabs.

---

## 2) Pre-flight checklist (one-time each sprint/drop)
1. Duplicate working sheets as backups (date-stamped).
2. Confirm active tabs/collections for this run only.
3. Freeze schema and header expectations for this cycle.
4. Confirm global config (locale, chain defaults, royalties, freeze policy) in Control/Globals tabs.
5. Verify filename conventions for images/animation files before data fill starts.

---

## 3) Data-entry order (minimum viable path)
Populate **manual fields first** in collection tabs (Manifest):
1. `token_id`, `Title/Name`, `Pantheon`
2. `Frame`, `Pallette`, `Format/Medium` (+ `Tier` if used)
3. `image_filename` (+ `animation_filename` where applicable)
4. `License_URL`, `edition_size` (+ `background_color` optional)

Then set Control-sheet project fields once per collection:
- `standard`, `chain`, `royalty_bps`, `royalty_recipient`, `contract_factory`, `collection_address`, etc.

Then Globals “set-and-forget” fields:
- `default_locale`, `secondary_locale`, `onchain_language`, `site_localization`.

---

## 4) Safe script running order (recommended)
Use this sequence to reduce collisions/redundancy risk:

### Phase A — Structure + health
1. `GHM_Canonicalize.gs`
   - Capture canonical headers from best tab.
   - Apply to all collection tabs.
2. `GHM_FastCompact.gs` (or `Stage_One_Sandbox.gs` equivalent)
   - Run main update pass (`RUN_updateAll_now`) and smoke/audit checks.

### Phase B — Data quality fixes
3. `Active_QA.gs`
   - Run active-tab QA fixers (tier casing, token range cleanup, sheet-level checks).
4. `GHM_Compact_Fix.gs` / `GHM_Rescue_Compact.gs`
   - Use only if compact layout/body-row integrity is off.

### Phase C — Language + localization
5. `GHM_Languages_SAFE_v2b.gs` (preferred safe pass)
6. `GHM_LANG_Prefill_JaZhHiIt.gs` and/or `GHM_Languages_L3_FixKit.gs` where needed.

### Phase D — Taxonomy + final polish
7. `Taxonamy.gs` (`GHM_Fill_Taxonomy_From_Mapping`) on all collection tabs.
8. `GHM_RC_AuditBodyRows.gs` for row-level consistency check.
9. `GHM_Audit_Final.gs` for final audit/sign-off.

---

## 5) QA gates to hit “export-ready”
A sheet is export-ready only when all gates pass:
- No missing required manual fields in active range.
- Auto-generated fields populated (`name_final`, `slug`, `attributes_json`, alt text fields, mime/bytes where expected).
- Control and Globals values present for target chain/standard.
- Header/schema consistency confirmed across all target collection tabs.
- Taxonomy and language columns completed per release scope.
- Final audit sheet/log contains no blocking errors.

---

## 6) Export packaging order (operations runbook)
1. Export collection CSVs from Manifest tabs.
2. Export Control docs (`Control`, `Globals`, `Tasks`, `Tiers`, etc.) as CSV snapshots.
3. Export Decision Tracker snapshot.
4. Generate metadata outputs (ERC721/ERC1155 JSON/CSV as required).
5. Build delivery folder structure:
   - `01_Sheets/`
   - `04_Docs/`
   - `05_Assets_Index/`
   - `03_Metadata_samples_/` (or final metadata folder)
   - project export directory (`06_GHM_NFT_Project/...`)
6. Add manifest index + checksums/version stamp for handoff package.

---

## 7) Final sign-off template (copy/paste)
- **Drop/Collection:**
- **Date/Version:**
- **Tabs included:**
- **Scripts run (in order):**
- **Blocking issues:** none / listed
- **QA owner approval:**
- **Ops approval:**
- **Export path/package name:**
- **Ready for publish/mint:** yes/no

---

## 8) Known risks and mitigations
- **Risk:** Redundant scripts overwrite expected output.
  - **Mitigation:** stick to ordered runbook; avoid ad-hoc scripts after final audit.
- **Risk:** Header drift across tabs.
  - **Mitigation:** always canonicalize first.
- **Risk:** Locale columns break compact views.
  - **Mitigation:** run SAFE language scripts first, then L3 fix only when needed.
- **Risk:** Manual data inconsistencies.
  - **Mitigation:** enforce pre-flight checklist and QA gate before export.

---

## 9) Quick “day-of-export” checklist
1. Confirm active collection tabs only.
2. Run Phases A→D in order.
3. Resolve audit blockers.
4. Export CSV/JSON/docs snapshots.
5. Build handoff package with version/date.
6. Record sign-off template.
7. Share package + Drive link + publish readiness status.

