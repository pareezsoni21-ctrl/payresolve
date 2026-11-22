# PayResolve v2
An independent product-management portfolio MVP. No live transactions, no banking connection, no credentials or personal data.

## Run on a Mac
Extract ZIP, open index.html in Safari or Chrome. If local browser restrictions occur, open Terminal in this directory and run `python3 -m http.server 8000`, then open http://localhost:8000.

## Test
With Node.js installed, run `node test.js` in the project directory. No dependencies. This regenerates BENCHMARK.csv and DEMO_DATA.json. Policy expectations are developer-authored from RBI Annex 4(a/b), not independently certified.

## Data
120 fixed synthetic cases at 2026-11-23. Later review dates project the same cases assuming no new resolutions. The simulator does not change the queue. No claims of real financial savings, adoption or accuracy.

## Files
index.html / style.css / app.js: interface; engine.js: business logic; test.js: test harness; BENCHMARK.csv: expected vs actual policy cases; DEMO_DATA.json: reproducible cohort; TEST_RESULTS.txt: automated results; CASE_STUDY.md: product strategy and PRD; USER_RESEARCH.csv: blank research template.

## Publish
Create a GitHub repository, upload these files to its root, and enable GitHub Pages under Settings → Pages → Deploy from a branch → main / root. Do not upload personal information or credentials.

Browser verification: eight smoke-check groups passed using Chromium with embedded assets; file:// navigation was blocked by the test environment. Try opening the extracted application locally, or use the local HTTP server instructions above.
