QUICK START — Merge MIME & BYTES into your manifest

1) Place these files next to your manifest CSV on your Mac:
   - ghm_merge_media_csv.py
   - ghm_merge_config.json
   - (Your existing) GHM_master_manifest.csv
   - (Your generated) media_scan.csv

2) Edit ghm_merge_config.json to match your column names if different:
   - key_columns.image_filename: the column holding your poster/still filename
   - key_columns.animation_filename: the column holding your video/3D filename
   - If you only have ONE column with a filename, set:
       "fallback_media_column": "media_filename"
     and leave image/animation filename keys as ""

3) Run from Terminal in that folder:
   python3 ghm_merge_media_csv.py
   # or with a custom config path
   python3 ghm_merge_media_csv.py /path/to/ghm_merge_config.json

4) Results:
   - GHM_master_manifest_UPDATED.csv (your updated manifest)
   - GHM_master_manifest_UPDATED_REPORT.txt (what was filled + any unmatched files)

Notes:
- The scan CSV must be created with exiftool, e.g.:
    exiftool -r -csv -FilePath -FileName -MIMEType -FileSize# . > media_scan.csv
- Matching is done by *basename* (filename only). If your manifest stores subfolders,
  it's still fine; we strip to basename for matching.
- Set "case_sensitive_match": true if you need exact case matching.
- Adjust "image_hints" / "animation_hints" if your naming differs (e.g., include "poster").
