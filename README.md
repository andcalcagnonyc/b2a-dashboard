# B2A Economy

An interactive dashboard that tracks the business-to-agent (B2A) economy: AI agents as users, buyers and customers of businesses.

It starts from a16z's essay [Welcome to the B2A Economy](https://www.a16z.news/p/welcome-to-the-b2a-economy) (Olivia Moore and Justine Moore, November 11, 2026), checks each number against its original source, adds independent context, and projects the main measures to 2030 under three scenarios.

**Live dashboard:** https://andcalcagnonyc.github.io/b2a (also on [claude.ai](https://claude.ai/artifact/3SZtPVT6e7Yy5D3jh7FhXu))

**Current edition:** 2026.11 (data as of November 11, 2026). Updated monthly.

Designed by [Andrea Calcagno](https://www.linkedin.com/in/andreacalcagno/).

The dataset and the methodology are open: free to use, share and adapt with credit (CC BY 4.0).

## Open dataset

Everything the dashboard shows is available in `data/`, rebuilt for each edition.

| File | Rows | Contents |
|---|---|---|
| `b2a-dataset.json` | | The complete dataset in one file |
| `signals.csv` | 26 | Actuals with value, period, evidence class, verification status and source |
| `series.csv` | 31 | Context time series |
| `model_scenarios.csv` | 82 | History and the conservative, base and accelerated paths for each modelled measure |
| `published_forecasts.csv` | 31 | Third-party forecasts as stated, with publisher and scope |
| `sectors.csv` | 12 | Impact scores by year for each industry |
| `sector_evidence.csv` | 34 | The evidence behind each sector |
| `technologies.csv` | 15 | Technology areas, maturity and mainstream year |
| `technology_traction.csv` | 28 | Funding and adoption items by technology |
| `sources.csv` | 91 | Every document cited |

Every CSV row carries its edition. Source ids link each table to `sources.csv`.

How the data is collected, classified, modelled and reviewed is in [METHODOLOGY.md](METHODOLOGY.md).

To cite: *B2A Economy dataset, edition 2026.11, by Andrea Calcagno. https://github.com/andcalcagnonyc/b2a. CC BY 4.0.* A `CITATION.cff` file is included.

Found an error? Open an issue with the figure and a link to the source you believe is right.

## What is in the dashboard

| View | Contents |
|---|---|
| Overview | The thesis, four headline measures, the article's six main numbers, how agents reach a business, who is expected to win, what could slow it down |
| Signals | 26 actuals from the article and from independent sources, plus 6 context series |
| Forecasts | 6 measures projected to 2030 with a scenario band, published third-party forecasts plotted on top, and 19 further published predictions |
| Sectors | 12 industries scored 0 to 5 by year for agent-driven change, with evidence for each |
| Technologies | 15 technology areas with maturity today and the estimated year each becomes mainstream |
| Methodology | Production pipeline, evidence classes, scenario model, scoring rubric, QA log, update log and all sources |

A bar fixed to the bottom of the page sets the year (2025 to 2030) and the scenario for every view at once. Two dials, adoption pace and trust-and-fraud friction, drive the modelled numbers. Every number opens its source, or its method if it is modelled. Six color palettes and light or dark mode are in the top bar.

## Evidence classes

Every number carries one of five marks.

| Class | Meaning |
|---|---|
| Reported | Stated by the company itself or measured by a named firm on its own data |
| Estimate | A third-party estimate of a current value, or a figure derived from one |
| Third-party forecast | A prediction published by a research firm or bank, shown as stated |
| Modelled | Computed by this dashboard's scenario model |
| Analyst judgement | A score assigned from the listed evidence using the published rubric |

## Repository layout

```
index.html              The page: layout, styles, charts and interaction. No build step.
data.js                 The dataset the page reads. The single source of truth.
data/                   The open dataset: one JSON file and nine CSV tables, generated from data.js.
METHODOLOGY.md          How the data is collected, classified, modelled and reviewed.
scripts/validate.mjs    Structural and arithmetic checks for data.js.
scripts/export.mjs      Builds data/ from data.js.
docs/UPDATE_RUNBOOK.md  Step-by-step procedure for the monthly edition.
LICENSE                 MIT, for the code.
LICENSE-DATA.md         CC BY 4.0, for the dataset and methodology.
CITATION.cff            Citation metadata.
```

The page loads [D3 7.9.0](https://d3js.org) from cdnjs and the Geist typeface from Google Fonts. Everything else is in the two files.

## Run it locally

Open `index.html` in a browser, or serve the folder:

```
python3 -m http.server 8000
```

Then visit http://localhost:8000.

## Hosting

The site is served by GitHub Pages from the root of the `main` branch, so every push to `main` redeploys it. There is no build step. The same two files are published as a claude.ai artifact.

After any edit to `data.js`, validate it and rebuild the open dataset:

```
node scripts/validate.mjs
node scripts/export.mjs
```

## The dataset

`data.js` sets one global object, `window.B2A`.

| Key | Contents |
|---|---|
| `meta` | Edition, as-of date, next update, current year, thesis and summary |
| `sources` | Every document cited: publisher, title, URL, publication date |
| `signals` | Actuals: value, period, source, evidence class, verification status, note |
| `series` | Context time series for the bar charts |
| `model` | Modelled measures: history, three paths for the next four years, published forecasts to plot, method text |
| `outlook` | Single-point published predictions, listed as stated |
| `sectors` | Industry scores by year with mechanism, evidence, winners, exposed players and risks |
| `tech` | Technology areas with layer, maturity, mainstream year, open problem, standards, companies and traction |
| `access`, `winners`, `risks` | The article's framework and the counter-evidence |
| `method`, `qa`, `changelog` | Methodology text, the review log for the edition, and the update log |

### Scenario model

Each modelled measure has a conservative, base and accelerated value for each of the next four years. The adoption pace dial (0 to 100) blends between paths on a log scale: 0 is conservative, 50 is base, 100 is accelerated. The friction dial shifts transaction measures later by up to 18 months. Values between yearly points are interpolated on a log scale. Paths are anchored on published forecasts where they exist, and the method text for each measure says where they are not.

## Monthly update

A scheduled task produces a new edition on the 11th of each month. It follows [docs/UPDATE_RUNBOOK.md](docs/UPDATE_RUNBOOK.md): refresh the figures, re-verify the previous edition, run an independent data review, validate, commit, and republish the live dashboard. Each edition records what changed in the update log inside the dashboard.

## How it is produced

Each edition is produced by a team of AI agents with separate roles (article analyst, actuals researcher, forecast researcher, sector and technology researcher, modeller, independent data reviewer, independent interface reviewer), directed and signed off by the project's human designer. The review findings for each edition are published in the Methodology view, including the items left open.

## Limits

- Several figures in the source article carry no link. The most likely source is shown and marked partly confirmed.
- Agent traffic shares come from single vendors measuring their own networks, not the whole web.
- Bot traffic and AI agent traffic are different things. Most automated traffic is still ordinary bots.
- Forecasters define agentic commerce differently (executed, influenced, orchestrated), which explains most of the spread in 2030 figures.
- Some figures were read through trade press because the original was paywalled or unreachable. The source entry names both.
- The dashed lines and the sector and technology scores are this project's own model and judgement.

This is an analytical product, not investment advice.

## Licence

- Dataset and methodology: [CC BY 4.0](LICENSE-DATA.md). Third-party figures remain the work of the publishers named in each source entry.
- Code: [MIT](LICENSE).

## Credits

Framework and starting data: [Welcome to the B2A Economy](https://www.a16z.news/p/welcome-to-the-b2a-economy) by Olivia Moore and Justine Moore, a16z. This project is independent and is not affiliated with or endorsed by a16z.

Design and direction: [Andrea Calcagno](https://www.linkedin.com/in/andreacalcagno/).
