First 10 items: file types + sizes to export

- `00_sheet_exports/` -> `06_GHM_NFT_Project/02_METADATA/exports/`
- `01_sources/` -> `06_GHM_NFT_Project/01_ASSETS/`
- `02_renders/images/` -> `06_GHM_NFT_Project/01_ASSETS/IMAGES/PREVIEWS/` + `.../THUMBNAILS/`
- `02_renders/video/` -> `06_GHM_NFT_Project/01_ASSETS/VIDEO/`
- `03_metadata/json/` -> `06_GHM_NFT_Project/02_METADATA/`
- `05_delivery/upload_ready/` -> `06_GHM_NFT_Project/06_EXPORT/<Marketplace>/`


	•	2) First 10 items: file types + sizes to export
	•	•	Preview image: JPG, sRGB, quality 80–85, usually 1600×1600 (or 2048×2048 for hero detail). 
	•	Thumbnail: JPG, sRGB, 512×512 (always square thumb).  Size targets:
	•	Preview < 1 MB
	•	Thumb < 120 KB  
If a token is video-based, also export:
	•	Poster JPG (for image) matching preview size, plus
	•	MP4 master (H.264 + AAC, 1080p30 or 4K30).


3) Correct filename when exporting
Use slug-based naming:13	§1
	•	<slug>-preview.jpg
	•	<slug>-thumb.jpg
	•	(video) <slug>-poster.jpg
	•	(video master) <slug>-4k30-h264-aac-v1.mp4 (or your 1080p naming variant) -1080p-h264-aac-v1.mp4


4) Photoshop metadata — what should you add?
For marketplace performance, do minimal metadata:
	•	Keep sRGB embedded profile
	•	Strip heavy EXIF / unnecessary metadata
	•	Optional: keep copyright/contact only if needed
That is exactly the current standard in your preset spec.

So in Photoshop export terms:
	•	Convert to sRGB
	•	Export JPG quality 80–85
	•	Minimize metadata payload


Title (exact artwork title)
Creator / Artist (your name / studio name)
Copyright Notice (e.g., © 2026 Mark Preston / Moda Digital Ltd.)
Rights Usage Terms (short usage statement)
Description (short artwork description)
Keywords (collection, character, pantheon, medium, edition type)
Creation Date
Contact / Website URL (project or artist site)
Project / Series (e.g., collection name)
