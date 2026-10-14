# ACMA Telco Lookup — GitHub Pages v1.1

A static web app for exact numeric ACMA Site ID lookup and place-name candidate search, with an interactive map and site links. It calls ACMA directly from the visitor's browser. No notebook, Python runtime, API key or server backend is required by this version.

## Publish on GitHub Pages

1. Extract this ZIP.
2. Sign in to GitHub and create a repository, for example `acma-telco-lookup`.
3. For GitHub Free, choose **Public**. Repository visibility and Pages-site access are separate settings; a private repository does not by itself make a Pages site private.
4. Upload `index.html` to the root of the repository (not inside an extra folder). Upload this README if desired. Commit to the `main` branch.
5. Open **Settings → Pages**.
6. Under **Build and deployment**, choose **Deploy from a branch**.
7. Choose branch **main** and folder **/(root)**, then click **Save**.
8. Wait for the Pages deployment to finish. Use the site address GitHub displays, normally `https://YOUR-USERNAME.github.io/acma-telco-lookup/`.
9. Enable **Enforce HTTPS** if it is not already selected.
10. Open the site and test it before embedding. The app shows no fabricated/example search results: it requests live records.

To update the app, replace `index.html` in the repository and commit. GitHub Pages redeploys it. No build tools, packages or GitHub Actions workflow file are needed for this branch-based setup.

## Update your existing repository

1. Extract this ZIP. It contains `index.html` and `README.md` directly.
2. Open your existing `acma-telco-lookup` repository on GitHub.
3. Choose **Add file → Upload files** and upload the replacement `index.html` (and optionally `README.md`) at the repository root.
4. Commit the changes to `main`. Keep the filename `index.html`.
5. Wait for the Pages deployment to finish and refresh the website. Its header should show **v1.1**.
6. The website URL stays the same. You do not need to create another repository or change the URL in Experience Builder.

### Refinements in v1.1

- Place name is the default search mode. Select Site ID when you know the numeric ACMA ID.
- Compact header and search bar; maximum candidates is under **Search options**.
- On wide screens (at least 1100 pixels), details sit beside the map. At narrower sizes they sit below it.
- The completed search term and actual search mode remain visible above the results, even when the inputs change.
- **Copy Site ID**, **Copy coordinates** and **Copy site summary** help transfer details. If clipboard access is blocked, selectable text appears for manual copying. Coordinate copying uses latitude, longitude in that order, with no datum transformation.
- **Look up this Site ID** reruns a selected place-name candidate as an exact numeric search. This identifies the ACMA record; it does not establish a Crown-account association.

## Embed in ArcGIS Experience Builder

1. Edit your saved Experience.
2. Add an **Embed** widget in the main page area.
3. Select **Embed by → URL**.
4. Paste the full HTTPS GitHub Pages address, including the repository path.
5. Use a wide main-page area. Start around 700 pixels high on desktop, or use the page's full available height. Narrow layouts put details below the map and may require a taller widget or scrolling.
6. Use Preview and run a place-name search. Select a candidate and check its map point and links.
7. Save and publish the Experience.

The embedded app has its own map and candidate list. Results do not automatically populate a separate native Experience Builder Map or Table widget. No Analysis widget, web-tool publishing, or Run web tools privilege is needed for this static embedded lookup.

## Search

- **Auto:** digits-only input is a Site ID; other text is a place-name search.
- **Site ID:** returns only an exact matching numeric ID; never substitutes a different API record. The KML endpoint is supplied as a download link; automatic KML fetching is not used because that endpoint did not return a browser cross-origin permission header during testing.
- **Place name:** searches ACMA's general text endpoint using a locality, road, property, landmark or site-name fragment. Shows candidate names, IDs and coordinates; the first candidate is not confirmed or auto-selected.
- **CN references:** search the known place associated with the CN reference. A CN reference alone is not a numeric ACMA Site ID and this app has no Crown-database connection.
- **Maximum candidates:** under Search options; accepts 1–200. If the returned results are capped, the app warns that the list may be incomplete. Increase the limit or narrow the term. It does not retrieve additional pages automatically.

Select a candidate in the list or click its map marker. Use **Show all sites** to return to the result extent. Topographic and aerial imagery basemaps are provided. Scroll-wheel zoom is off so the embedded map does not trap page scrolling; use the map zoom buttons or touch gestures.

## Important accuracy details

ACMA JSON coordinates do not carry a datum field in the response inspected for this release. The browser map uses their supplied latitude/longitude numerically for visual location; no precise GDA94/WGS84 transformation is performed. The interface states this limitation. Use authoritative coordinates and a verified datum for precise spatial work.

ACMA text search can match a road name or site description outside the locality you intended. Compare names and map positions with your known site/Crown context. An exact Site ID identifies an ACMA record; it does not prove a Crown-account association.

Candidates without usable coordinates remain in the list but have no map marker. A missing basemap does not prevent searching or displaying the textual results.

## Verification

During preparation:
- Live ACMA JSON requests succeeded for numeric and place-name searches.
- The JSON endpoint returned `Access-Control-Allow-Origin: *` for a request using a GitHub Pages-style Origin header.
- Parser, exact-ID handling, candidate counts, result-limit warnings, missing coordinates and error routing were checked with live captured responses.
- Application scripts passed JavaScript syntax checks.
- Version 1.1 passed 42 core lookup checks and 28 UI interaction checks using a DOM harness, covering search context, candidate/exact labels, the ID shortcut, copy text and blocked-clipboard fallback.

The original version was shown working on the user's standalone page. Version 1.1 layout rendering and Experience Builder iframe execution still need checking in the user's browser. The DOM harness does not test browser layout or iframe permissions. Confirm the checklist below after updating.

## Check after publishing

1. Search **Gillamatong** using **Place name**. During preparation ACMA returned four sites; counts may change.
2. Search **Braidwood** using **Auto**. Compare the list and map. Some matches may be outside Braidwood.
3. Search **9723** using **Site ID**. During preparation it returned the Services Site at Mt Gillamatong, approximately `-35.455206, 149.771452`.
4. Search an unlikely place name. Confirm the app shows no candidates and clears old markers.
5. Enter **CN075**. Confirm it asks for the associated known place name.
6. Try a small candidate limit, e.g. **5** for Braidwood. Confirm the incomplete-results warning appears.
7. Repeat one search inside Experience Builder. Check candidate selection, map tiles and external links there too.
8. Change the input and mode after a search; confirm the results still identify the term and mode that produced them.
9. Select a place-name candidate, then choose **Look up this Site ID**. Confirm it becomes an exact result and clears the other candidate points.
10. Test all three copy buttons, including inside the embed. If automatic copy is blocked, select the displayed text and press Ctrl+C (Command+C on Mac).
11. Resize the embed and preview desktop and mobile layouts. Check that details and map controls remain accessible.

## Troubleshooting

- **404 on Pages:** check `index.html` is at the top level of the selected publishing folder and the URL includes the repository name.
- **Search cannot reach ACMA:** test the page outside Experience Builder. Browser CORS rules, network restrictions or ACMA availability may differ from the preparation tests. The app reports errors without manufacturing a result.
- **Map unavailable:** the Leaflet CDN or Esri tile service may be blocked. Site lists and coordinates remain usable if the map library cannot load.
- **ACMA JSON unavailable:** this static version reports the failure. It does not perform the notebook's server-side KML fallback; the KML service did not allow cross-origin browser fetching during preparation.
- **Links do not open inside the embed:** test from the standalone Pages URL; iframe popup policies may vary.
- **Older code after updating:** check the Pages deployment finished, then refresh or hard-refresh the browser.

## Dependencies and attribution

- Leaflet 1.9.4, loaded from unpkg with the integrity values in Leaflet's official quick-start documentation.
- Esri World Topographic Map and World Imagery tile services, with visible attribution in the map.
- Australian Communications and Media Authority, Register of Radiocommunications Licences, for site records and KML.

The app does not include passwords, ArcGIS tokens, internal Crown records or the project blueprint files. It stores search results only in the current page's memory.

## References

- https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site
- https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site
- https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https
- https://doc.arcgis.com/en/experience-builder/latest/configure-widgets/embed-widget.htm
- https://leafletjs.com/examples/quick-start/
