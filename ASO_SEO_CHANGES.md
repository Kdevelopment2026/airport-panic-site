# ASO + SEO changes (branch `seo-aso-update`, 2026-10-26)

Nothing has been pushed, uploaded or submitted. The App Store metadata edits are in the app repo at
`airport_panic/ios/fastlane/metadata/en-GB/` and are NOT part of this git commit (that folder is not under git).

## 1. App Store metadata (en-GB)

Counts are characters / UTF-8 bytes, measured with Python. None of the files ends with a newline, same as before.

| Field | Before | After |
|---|---|---|
| name.txt (max 30) | `Airport Panic: Family Rush` (26) | unchanged (owner kept the name; "travel" moved into keywords instead of "dash") |
| subtitle.txt (max 30) | `Time management travel game` (27) | `Packing & time management game` (30) |
| keywords.txt (max 100 bytes) | `packing,suitcase,luggage,holiday,vacation,flight,plane,airline,mini games,kids,arcade,timer,dash` (96) | `suitcase,luggage,holiday,vacation,flight,plane,airline,offline,security,boarding,mini,puzzle,dash` (97) |
| promotional_text.txt (max 170) | `Five airport mini-games, one boarding clock. Pack the case, clear security, sprint to the gate and get all four Bramleys on board. Levels 1-10 free. No ads, ever.` (162) | `Pack the case, clear security, dash to the gate and board before the doors close. Five airport mini-games, one countdown. Levels 1-10 free, no ads, plays offline.` (162) |
| description.txt (max 4000) | 2243 | 2345 |
| release_notes.txt | not touched | not touched |

**Why these choices**
- Words indexed from the name and subtitle: airport, panic, family, travel, packing, time, management, game. The name and subtitle together cover search phrases like "family travel game", "packing game", "airport game" and "time management game".
- "Packing" moved out of keywords and into the subtitle, where it is weighted more heavily. Suitcase packing puzzles such as Pack Master and Luggage Pack show this is a real search intent.
- Keywords no longer repeat any word from the name or subtitle. Removed: `packing`, `kids` (it implies Kids Category), `timer`, `arcade` and `mini games` (now just `mini`, which pairs with "game" from the subtitle). Added: `offline` ("offline games" is a large query and it is true), `security`, `boarding`, `puzzle`.
- Name and subtitle contain no price claims and no competitor names.
- Search volumes were not measured: no paid ASO tool was used. Competitor check was a quick web search only (Airport Life 3D, Airport Scanner, Pack Master, Luggage Pack, An Airport Game).

**Description, new first lines (the part visible before "more")**
```
Can you get the whole family on the plane before boarding closes?

Pack the suitcase, drop the bags, clear security, dash to the gate and board in time. Five airport mini-games, one countdown, 2-5 minutes a level.

Levels 1-10 free. No adverts. Plays offline.

Airport Panic is a fast, funny time management game about family travel chaos.
```
Tip jar line, added at the end of the PLAYS ANYWHERE bullet:
`... One optional purchase unlocks levels 11-20, the Holiday Rush finale and every future level. Full Game owners can also leave an optional tip to support future updates.`
The rest of the description is unchanged.

## 2. Marketing site

| File | Change |
|---|---|
| `index.html` | Was a copy of `support.html` titled "Support". It is now the home page. New title, meta description, canonical, `apple-itunes-app` smart banner (app-id=6809162851), Open Graph and Twitter `summary_large_image` tags, favicon and apple-touch-icon, and JSON-LD `["VideoGame","MobileApplication"]` (operatingSystem iOS, offer price 0 GBP, no rating). Visible "Download on the App Store" button plus a second App Store link in the copy. H1 "Airport Panic". H2s: "A family travel game about making your flight", "Five airport mini-games, one boarding clock", "Free to start, no ads, plays offline", "Support". The support email is still on the home page, because `support_url.txt` points at the site root. |
| `support.html` | Same support, FAQ and delete-data content as before. New title "Airport Panic Support & FAQ", its own meta description, a canonical pointing to itself (so it no longer duplicates the home page), OG tags, smart banner, App Store button, H1 "Airport Panic Support", and a Home / Support / Privacy nav. |
| `privacy.html` | Still indexable (no `noindex`). Added title, meta description, canonical, OG tags and the new nav. Tip-jar wording: the "In-app purchase" heading became "In-app purchases" and now says "The optional Full Game unlock, and any optional tip, is processed entirely by Apple…". "The one optional purchase is protected by…" became "Optional purchases are protected by…". The effective date is unchanged, because data practices are unchanged. |
| `style.css` | Added only `.lede` and `.store` (the App Store button). Nothing existing was changed. |
| `robots.txt` (new) | Allows everything and lists the sitemap. **Caveat:** crawlers only read `robots.txt` at the host root (`kdevelopment2026.github.io/robots.txt`), so this file under `/airport-panic-site/` is ignored by Google. It does no harm. |
| `sitemap.xml` (new) | Lists the home, support and privacy URLs, lastmod 2026-10-26. Valid XML. |
| `assets/og-image.jpg` (new) | 1200×627 JPEG made from `marketing/exports/airport-panic-linkedin-1200x627.png`. |
| `assets/icon-512.png`, `apple-touch-icon.png`, `favicon-48.png` (new) | Resized from the 1024 app icon. |

Checks done: the JSON-LD parses with Python `json`, the sitemap parses as XML, HTML tags balance on all 3 pages, and the home page renders at 400px and 1280px in a headless browser. Not checked: Google Rich Results Test, Facebook/X card previews (these need the live URL).

Note: without `aggregateRating` or `review`, Google will not show a software-app rich result. The schema still helps Google understand the page. No rating was invented.

## 3. Next steps that need you
1. **Review and push** the branch: `git push -u origin seo-aso-update`, then merge to `main` (GitHub Pages serves `main`).
2. **Google Search Console:** add a URL-prefix property for `https://kdevelopment2026.github.io/airport-panic-site/`. Verify it with the HTML-tag method, which means adding the `google-site-verification` meta to `index.html`. Then submit `sitemap.xml` and request indexing of the home page.
3. After deploy, run https://search.google.com/test/rich-results and https://validator.schema.org on the live URL. Check the share preview by pasting the link into Slack, iMessage or X.
4. **Old name still in other places:** the OG image (and the other marketing exports) show "Airport Panic: Family Rush". So does the VoiceOver label in `lib/features/home/home_screen.dart:214`. Update them if you keep the new store name.
5. Upload the metadata with your normal fastlane flow when you submit the version that includes the tip jar. The description's tip line and the privacy page's tip wording only become fully accurate once that version is live, so hold the site merge until then if you want strict accuracy. The privacy wording says "any optional tip", so it is not false before then.
6. Optional: if you ever create a `kdevelopment2026.github.io` user-site repo, put a `robots.txt` there that points to this sitemap.
