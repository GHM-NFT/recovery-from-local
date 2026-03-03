What the “link” usually is
For each NFT, marketplaces show a clickable Website / External link that comes from your metadata’s external_url field.
Best practice
	•	Point external_url to a public, branded page for that token:
	◦	Example patterns:
	▪	https://yourdomain.com/nft/ethereum/0xCONTRACT/1234
	▪	https://yourdomain.com/greek/poseidon/1234
	•	What to show there:
	◦	Hero image/video, story/meaning, traits, artist bio, license, collection context, socials, press kit.
	◦	A prominent button that opens your token-gated unlockables portal (details below).
Don’t use ipfs:// for external_url—keep that as a normal https:// page you control. Continue using ipfs:// for image and any animation_url.
How collectors access unlockables
There are two common patterns. Use one (or both):
	1	Marketplace-native “Unlockable content” (e.g., OpenSea)
	◦	You paste private text/links per token during listing.
	◦	Only the owner (and you) can see it on the marketplace page after purchase.
	◦	Pros: quick; no dev work. Cons: not portable across sites; harder to update/track.
	2	Your own token-gated portal (recommended for premium tiers)
	◦	https://yourdomain.com/unlock/ethereum/0xCONTRACT/1234
	◦	The page asks the wallet to connect and verifies ownership (ownerOf / balanceOf) before revealing links (6K/8K/10K, video, VR, COA, etc.).
	◦	Pros: works across marketplaces, lets you track access, swap links, rotate expiring URLs, and offer future perks.
Important security notes
	•	Never put private unlockable URLs in public on-chain JSON.
	•	If you must host large files in the cloud, serve them via short-lived signed URLs (e.g., S3/GCS) after ownership is verified. “Hidden IPFS” isn’t secure once the CID leaks.
	•	Your existing per-tier unlockable .txt (the ones our script generates) is perfect for:
	◦	OpenSea’s “unlockable content” box (paste the text there), or
	◦	The body of your token-gated portal page after auth.
Simple, scalable URL scheme
Use consistent paths for both the public page and the gated page:
	•	Public details page (goes in external_url): https://yourdomain.com/nft/<chain>/<contract>/<tokenId>
	•	Gated unlockables page (button on the public page): https://yourdomain.com/unlock/<chain>/<contract>/<tokenId>
You can also support a slug route for SEO:
	•	https://yourdomain.com/nft/<slug> → redirects to the canonical <chain>/<contract>/<tokenId>.
How this fits your master + script
	•	Keep filling external_url in the sheet, or let’s derive it automatically.
	•	We already generate per-token unlockable text from your tier templates. You can:
	◦	Paste that into OpenSea’s unlockable field, or
	◦	Show the same content behind your /unlock/ page.
If you want automation, I can update the generator to:
	•	Add two optional columns:
	◦	external_url_base (e.g., https://yourdomain.com/nft/ethereum/0xCONTRACT)
	◦	unlockable_portal_base (e.g., https://yourdomain.com/unlock/ethereum/0xCONTRACT)
	•	Auto-set:
	◦	external_url = <external_url_base>/<token_id> (only if your sheet’s external_url is blank)
	•	Insert the unlockable portal URL into each generated unlockable .txt (at the top), like: Access your unlockables: https://yourdomain.com/unlock/ethereum/0xCONTRACT/1234
Quick choose-your-path
	•	Fastest: Use OpenSea’s unlockable content box and point external_url to your token page.
	•	Best long-term: Token-gated portal + external_url to your token page + (optionally) also use OpenSea’s box to link back to your portal.
If you want the auto-URL columns added and wired into the script + workbook, say “add the auto-URL columns,” and I’ll ship an updated master + generator that fills external_url and drops the portal link into each unlockable file.



