# Visible Nature Atlas: Grasses and sedges of Montserrat

This is a volume in the **Visible Nature Atlas** series — an open, data-driven collection of
biodiversity books covering the flora and fauna of specific regions worldwide.

## About this atlas

This atlas documents the **grasses (Poaceae) and sedges (Cyperaceae)** of **Montserrat**, a small
British Overseas Territory in the Caribbean Lesser Antilles. Despite its modest size (102 km²),
Montserrat harbours a rich diversity of native and introduced grass and sedge species, collected
by herbaria and field observers over more than a century.

## Observations on Montserrat

The map below shows all georeferenced GBIF occurrence records from this dataset, coloured by
species. Click any marker to see the species name, collecting institution, and a link to the
original GBIF record.

```{raw} html
<iframe src="maps/montserrat_overview.html"
        width="100%" height="520px"
        frameborder="0" scrolling="no"
        style="border-radius:8px; margin-bottom:0.5rem;"></iframe>
```

Map data © [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors.
Occurrence data from [GBIF](https://www.gbif.org/).

## Data sources

All data is sourced from open, freely available biodiversity databases:

| Source | Role |
|--------|------|
| **[GBIF](https://www.gbif.org/)** | Occurrence records — herbarium specimens, field observations, citizen science |
| **[Wikidata](https://www.wikidata.org/)** | Taxonomic identifiers, institutional metadata, licence information |
| **[Wikipedia](https://en.wikipedia.org/)** | Species introductions (first paragraph of the English article) |
| **[iNaturalist](https://www.inaturalist.org/)** | Photographs from field observations |
| **[OpenStreetMap](https://www.openstreetmap.org/)** | Base map tiles for all interactive maps |

## How this book is compiled

This atlas is generated entirely from open data using a reproducible, automated pipeline.
No data is entered by hand — every page is produced by running two Jupyter notebooks.

```
GBIF download → inspectData.ipynb → generateTaxaPages.ipynb → jupyter-book build → GitHub Pages
```

**Step 1 — `inspectData.ipynb` (data pipeline)**

1. Loads the raw GBIF occurrence download (`occurrence.txt`, `multimedia.txt`)
2. Resolves institution names to Wikidata Q-identifiers via the Wikidata SPARQL endpoint
3. Matches species names to Wikidata taxon entities and retrieves kingdom classification
4. Fetches English Wikipedia introductions for each species
5. Constructs an RDF knowledge graph linking every observation to its taxon, institution,
   media files, and licence — serialised as `gbifMontserrat.ttl`
6. Downloads the full Wikidata RDF descriptions of all referenced entities
   (`wikidataMontserrat.ttl`)
7. Queries the local RDF graph with SPARQL to extract structured taxon and collection data
8. Generates one markdown page per collecting organisation (`organisation/`) and
   updates `_toc.yml`

**Step 2 — `generateTaxaPages.ipynb` (species pages + maps)**

1. Parses `gbifMontserrat.ttl` to extract the list of species and their observations
2. For each species, queries the GBIF occurrence API for up to 300 worldwide georeferenced records
3. Generates an interactive [Folium](https://python-visualization.github.io/folium/) /
   OpenStreetMap map per species, saved as `maps/<Species_name>.html`
4. Fetches the Wikipedia introduction for each species
5. Writes a complete markdown chapter per species (`taxa/`) with Wikipedia text,
   the global distribution map, and all Montserrat observation photographs
6. Generates the Montserrat overview map (`maps/montserrat_overview.html`)
7. Updates `_toc.yml` with the full list of generated pages

**Step 3 — `jupyter-book build .`**

[Jupyter Book](https://jupyterbook.org/) (v1, Sphinx-based) converts all markdown pages
into a static HTML website, resolving cross-references, building the navigation sidebar,
and embedding the Folium map iframes.

**Step 4 — GitHub Actions → GitHub Pages**

On every push to `main`, a GitHub Actions workflow runs all three steps automatically
and deploys the result to GitHub Pages. The full workflow is in
`.github/workflows/build-book.yml`.

## Structure of this book

**Part 1 — Collections:** Observations organised by the herbaria, botanical gardens,
and field observers that contributed records from Montserrat.

**Part 2 — Taxa:** One chapter per species, each containing:
- A brief description from English Wikipedia
- An interactive map of worldwide GBIF records (OpenStreetMap)
- Photographs from the Montserrat dataset, with licence information

## Authors

- **Sofie Meeus** — [Meise Botanic Garden](https://www.botanicgarden.be/)
- **Quentin Groom** — [Meise Botanic Garden](https://www.botanicgarden.be/)
- **Andra Waagmeester** — [Gene Wiki](https://www.wikidata.org/wiki/User:Andrawaag)

## Reproducibility

All source code, notebooks, and configuration are publicly available on
[GitHub](https://github.com/VisibleNatureAtlas/Grasses-and-sedges-of-Montserrat).
The raw GBIF data download is included in the repository (`data/`).
Running the two notebooks in order reproduces the entire atlas from scratch.

```{note}
The interactive maps require JavaScript to be enabled in your browser.
Each species map shows worldwide records to illustrate the species' global range,
not only its occurrence on Montserrat.
```
