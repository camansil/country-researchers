# Country Researchers

A catalogue of **evidence producers by country**: researchers based in a country who have published empirical studies conducted there with explicit relevance for public policy or programmes, shown on a map with a searchable list and a profile per person. It is a separate product from the [Policy Window Monitor](https://github.com/camansil/policy-windows); matching researchers to an open policy window is done by a person.

Countries: **Chile** (first catalogue, November 2026) and **Colombia** (in preparation). Switch with the menu at the top right.

## Publish on GitHub Pages

In **Settings → Pages**, set **Source** to *Deploy from a branch*, branch `main`, folder `/ (root)`. The site appears at `https://camansil.github.io/country-researchers/`.

## Structure

```
index.html                       the site (no build step); reads countries/<iso>/catalogue.json and outline.json
countries/<iso>/raw/             search exports (RIS) from each database
countries/<iso>/records.json     parsed and deduplicated records
countries/<iso>/screening_*.jsonl   screening decisions (model pass, to be checked by a person)
countries/<iso>/openalex_*.json  author, ORCID and institution data from OpenAlex
countries/<iso>/catalogue.json   people, institutions and works shown on the site
countries/<iso>/people.csv       the same people as a table
countries/<iso>/outline.json     the country's outline for the map
scripts/parse_ris.py <iso>       parse and deduplicate the exports
scripts/enrich_openalex.py <iso> link records to OpenAlex (runs in GitHub Actions: Actions → OpenAlex enrichment)
scripts/build_catalogue.py <iso> write catalogue.json and people.csv
docs/                            protocol (English and Spanish) and screening criteria
```

## Pipeline for a country

1. Run the searches in the protocol (`docs/`) and save the exports in `countries/<iso>/raw/`.
2. `python3 scripts/parse_ris.py <iso>`.
3. Screen titles and abstracts against the two criteria (`docs/SCREENING_v03.md`).
4. Run the **OpenAlex enrichment** action with the country code. It needs the repository secret `OPENALEX_API_KEY` (Settings → Secrets and variables → Actions).
5. `python3 scripts/build_catalogue.py <iso>`, then add the country to `COUNTRIES` in `index.html`.

## Chile: known gaps

- About 330 included records have no DOI and are not yet linked to authors (mostly LILACS, Overton and older items).
- Screening is a single model pass; spot-check before using the catalogue outside the team.
- The first searches (18 Nov 2026) used Web of Science, LILACS and Overton with "Chile" and "policy"; the protocol widens them (main cities, more policy terms, more databases).
- Projects (ANID) are not linked yet.
