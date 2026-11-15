<div align="center">

# 🔴 Mars Atlas

### Explore Jezero. Plan a route. Understand the terrain.

**A manual-first Mars mission-planning workspace: a 3D planet built on NASA's own model, a 2D map of real orbiter imagery, route analysis you can explain in plain words, and a lab of 25 small planning models.**

[![Verify](https://github.com/mari0-th/InfotronX-Horizon/actions/workflows/verify.yml/badge.svg)](https://github.com/mari0-th/InfotronX-Horizon/actions/workflows/verify.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
![No build step](https://img.shields.io/badge/build-none-brightgreen)
![Vanilla JS](https://img.shields.io/badge/JavaScript-ES%20modules-f7df1e)
![Three.js](https://img.shields.io/badge/3D-three.js-black)
![NASA data](https://img.shields.io/badge/data-NASA%20%C2%B7%20USGS-0b3d91)

  **[▶ Open Mars Atlas](https://mars-atlasv2.netlify.app/)** · [Releases](https://github.com/mari0-th/InfotronX-Horizon/releases) · [Changelog](CHANGELOG.md) · [Credits](CREDITS.md) · [Report a bug](https://github.com/mari0-th/InfotronX-Horizon/issues/new?template=bug_report.md)

*Built by **InfotronX Horizon** for the NASA Space Apps Challenge 2026 · "Interplanetary Survival Guide: Martian Map"*

</div>

> ⚠️ Mars Atlas is an independent educational project. It is a planning aid, **not** a navigation tool, and it is **not** certified, endorsed or approved by NASA.

---

## Why Mars Atlas

Planning a walk on Mars is a chain of small decisions: where to go, which way, how steep, how much oxygen, what if the radio drops. Most tools show you a map *or* a number. Mars Atlas puts them together and **explains every number in a sentence built from your own route**, labelled so you always know what it rests on:

| Label | Meaning |
| --- | --- |
| **Measured** | Comes straight from NASA or USGS data |
| **Calculated** | Computed from a formula on this page, from measured data |
| **Illustrative** | A scenario or a simple model, not a measurement |
| **Your note** | Something you wrote |

Nothing is called "real" unless it is. The MOLA relief on the 3D planet is 8-bit (about 115 m per grey level), so it says *approximate*. The 20 m CTX height model only covers a clip near the Jezero delta, so outside it slopes are *unknown*, never "flat".

## Highlights

### 🌍 A planet you can fly around
- **NASA's own 3D model of Mars** (NASA/JPL-Caltech), made denser and lifted by MOLA heights, with a slider for the exaggeration.
- **Switch the surface:** the NASA model, Viking colour, MOLA shaded relief or MOLA colour relief. Close up, it borrows the sharper Viking map by itself.
- **29 real places** (rover and lander sites, volcanoes, canyons, basins) with facts and NASA photos. Click a glowing pin.
- **Routes anywhere on the planet**, following the surface, with great-circle distances on Mars's real radius.
- **Fly over your route** with a height profile that moves in sync, drop a figure onto the map for a **surface view** like street view, and tint the ground by **slope**.
- A **data-quality line under the pointer** says what the numbers there rest on, in 2D and in 3D.

### 🗺️ A map built from real imagery
- NASA Mars Trek layers: Viking colour, MOLA relief, CTX (6 m) and HiRISE (25 cm) around Jezero, plus a NASA colour-relief layer.
- Draw routes, move and reorder waypoints, add notes, photos and extra links between waypoints. A dot travels along the active route in the *Flowing* line style.

### 🧭 Plan, then understand
- **Explore → Plan → Analyze:** one panel at a time, with the map taking the rest of the screen. **All tools** keeps the full science workspace, with a search across every tool.
- **Analyze in eight explained steps:** size, height, legs, comparison, 3D, decision checklist, questions, safety. A slider trades **distance against relief** and names the penalty (about 10.6 on the benchmark) where the best route changes.
- **A repeatable benchmark** on the Jezero delta: three routes, one start, one end, numbers re-checked by the test suite.
- **Safety step:** robots that could go along, what to pack, the point of no return, and the Earth–Mars message delay (3 to 22 minutes one way).

### 🧪 The Planning Lab
Twenty-five small models for mission ideas, each with an example already filled in. Change the numbers, press **Run**:

| | | |
| --- | --- | --- |
| Value plan under a time budget | Instruments and kit weight | Geological variety |
| *Why is there no route?* | Backup plans with triggers | Does the best route stay the best? |
| What to measure next | Navigation without GPS | Image / height-model alignment |
| Turning radius | Vehicle width along the route | Side slope and tipping |
| Wheel slip | Two rovers in a narrow pass | Where two teams meet |
| Radio relay from line of sight | Time windows | Battery over time |
| Equipment temperature | Depots and resupply | Samples and contamination |
| Data budget | Recovery after an uncertain landing | Change between two pictures |
| Local 3D points on the planet | Export for a small educational rover | |

These are planning aids built from the numbers you give. They are not measurements and not a safety approval.

### 🎓 Made to be learned
A tutorial with the logo as its guide, a "how to choose a route" guide, a welcome page that says what each NASA source is used for and how good it is, and preferences kept in your browser (nothing is sent anywhere).

---

## How it works

1. **Explore:** start on the welcome screen, look around, pick a map or a 3D surface.
2. **Plan:** click to place waypoints, drag to adjust, add observations and photos.
3. **Analyze:** follow the explained steps, move the slider, open the Lab.

Sign in once and your missions save automatically. Guests can use everything, but nothing they draw is saved and they are warned before leaving.

## The mathematics

Every number in Mars Atlas is either **measured** (from NASA or USGS data), **calculated** from the formulas below, an **illustrative scenario**, or your own note. The code that implements each formula is named next to it, and the worked examples are checked by an automated test (`tests/readme-math.test.mjs`), so this page cannot drift from the program.

### 1. Distance on Mars (`src/geo.js`)

Mars is treated as a sphere of radius $R = 3\,396\,190\ \text{m}$. For two points with latitudes $\varphi_1,\varphi_2$ and longitudes $\lambda_1,\lambda_2$, the great-circle distance uses the haversine formula:

$$a = \sin^2\!\frac{\varphi_2-\varphi_1}{2} + \cos\varphi_1\,\cos\varphi_2\,\sin^2\!\frac{\lambda_2-\lambda_1}{2}$$

$$d = 2R\,\arcsin\!\sqrt{a}$$

The length of a route with waypoints $P_0,\dots,P_n$ is the sum of its legs:

$$D = \sum_{i=1}^{n} d(P_{i-1},P_i)$$

Worked checks:

| Case | Result |
| --- | --- |
| $1^\circ$ of latitude, $2\pi R/360$ | $59\,274.7\ \text{m}$ |
| $1^\circ$ of longitude at $60^\circ$ latitude (half of the equator value) | $29\,637.1\ \text{m}$ |
| Two points in Jezero, $(18.4446^\circ,77.4509^\circ)$ and $(18.4500^\circ,77.4600^\circ)$ | $603.5\ \text{m}$ |
| Antipodal points, $\pi R$ | $10\,669.4\ \text{km}$ |

Mars is slightly flattened (polar radius about $0.994$ of the equatorial one). The distance formula ignores this; the error is below one percent. The 3D globe uses the flattened shape.

### 2. Slope and climb (`src/terrain.js`, `src/terrain-profile-v2.js`)

Between two samples of the height model, 20 m apart or more, the grade is

$$g = \frac{h_B-h_A}{d}\times 100\ \%$$

so a grade of $7.3\,\%$ means $7.3\ \text{m}$ of rise for every $100\ \text{m}$ travelled. Total climb and descent add up every positive and every negative change along the profile:

$$A=\sum \max(0,\Delta h),\qquad H=\sum \max(0,-\Delta h)$$

Heights come from the USGS CTX terrain model (20 m cells) and are relative to the Mars datum. Where the model has no data, the answer is "unknown", never "flat".

### 3. Route cost for the terrain planner (`src/terrain.js`, `src/route-decision.js`)

On the height grid, moving between neighbouring cells of length $d$ and grade $g$ costs

$$c = d\left(1+\lambda\,\frac{|g|}{100}\right) = d + \lambda\,|\Delta h|$$

because $\frac{|g|}{100}\,d=|\Delta h|$. Summed along a path this gives the cost shown as *Why this route?*:

$$C = D + \lambda\,(A+H)$$

$\lambda$ is the penalty, in metres of cost for each metre of height change. It is $0$ for *minimise distance*, $6$ for *balanced* and $24$ for *gentle*. Cells steeper than the chosen limit (18 % by default) and cells in exclusion zones are not entered. A\* finds the cheapest path among the cells it can reach, so it is the best of the routes the grid allows, not a proven safe route.

Example: a $100\ \text{m}$ step with a rise of $5\ \text{m}$ (grade $5\,\%$) and $\lambda=6$ costs $100\,(1+6\cdot 0.05)=130$, which equals $100+6\cdot5$.

Two routes are compared on the pair $(D,\ A+H)$. A route *dominates* another when it is no longer and has no more height change, and is strictly better in at least one of them. The routes nobody dominates form the Pareto set.

### 4. Time, oxygen and energy (`src/resources.js`)

An illustrative scenario with walking speed $v$ (km/h), time at stops $t_s$ (hours), oxygen use $r_{O_2}$ (kg/h) and power $P$ (W). If the way back retraces the route, the travelled distance is $D_{\text{travel}}=2D$, otherwise $D$.

$$t=\frac{D_{\text{travel}}/1000}{v}+t_s,\qquad m_{O_2}=t\,r_{O_2},\qquad E=t\,P$$

Usable amounts keep a reserve of $\rho\,\%$:

$$X_{\text{usable}} = X_{\text{available}}\left(1-\frac{\rho}{100}\right)$$

Defaults: $v=2.4$ km/h, $t_s=0.25$ h, $r_{O_2}=0.12$ kg/h, $P=280$ W, $1.2$ kg of oxygen, $2200$ Wh of battery, $\rho=25\,\%$, $8$ h maximum. The scenario is a planning illustration, not measured telemetry, and it ignores terrain.

Worked check, a $4.2\ \text{km}$ route with the way back: $D_{\text{travel}}=8.4$ km, $t=8.4/2.4+0.25=3.75$ h, $m_{O_2}=0.45$ kg against $0.9$ kg usable, $E=1050$ Wh against $1650$ Wh usable, and $3.75$ h against $6$ h usable.

### 5. Risk measures in the advanced lab (`src/advanced-math.js`)

The lab draws seeded synthetic variations of the resource inputs and reports the mean, the value at risk and the conditional value at risk at confidence level $\alpha$:

$$\mathrm{VaR}_\alpha = \text{the } \lceil \alpha N\rceil\text{-th smallest of } N \text{ samples},\qquad \mathrm{CVaR}_\alpha=\frac{1}{(1-\alpha)N}\sum_{\text{worst tail}} x$$

These describe hypothetical distributions. They are **not** NASA-calibrated probabilities and say nothing about the safety of a real extravehicular activity.

## Data and its limits

| Source | Used for | Quality |
| --- | --- | --- |
| NASA Viking MDIM 2.1 colour mosaic | Colour map in 2D and 3D | Measured imagery; tiles stop at zoom 7, so close zoom is enlarged, not new |
| NASA MRO CTX and HiRISE (Mars Trek) | Detail around the Jezero delta | Measured imagery, footprint only |
| NASA/GSFC MOLA elevation | 3D relief; heights outside Jezero | 8-bit, about 115 m per level: **approximate** |
| USGS CTX DTM, 20 m (Fergason et al., 2020, [DOI 10.5066/P906QQT8](https://doi.org/10.5066/P906QQT8)) | Slopes, profiles, route planning | Calculated; a clip near the delta, **not** a full Jezero survey |
| NASA/JPL-Caltech 3D model of Mars | The planet in 3D | Model; relief is illustrative |
| NASA Image and Video Library | Photos of the 29 places | NASA photos |

The scientific analyses are educational approximations. They are not mission-operational hazard measurements, medical advice or safety approvals. Scenario assumptions must not be confused with source measurements. Map layers are visual context, not certified terrain safety maps. More in [NASA_DATA.md](NASA_DATA.md), [Science validation](docs/SCIENCE_VALIDATION.md) and [Map engine](docs/MAP_ENGINE.md).

## Current status

| Area | Status |
| --- | --- |
| Code and automated tests | Available; GitHub Actions runs on pushes to `main` |
| 3D planet, 2D map, Analyze, Lab | Implemented; browser testing by the team in progress |
| Mission saving | Guests: not saved. Signed-in: automatic cloud save with a device copy, plus exports |
| Team access | Cloud save with per-mission viewer/editor roles; no automatic invitation emails |
| Live simultaneous editing | **Not implemented** (snapshots are opened and saved explicitly) |
| Global chat | Disabled until moderation is staffed |
| Rover or astronaut safety certification | **Not provided** |

## Run it locally

Requires **Git** and **Node.js 22+**. The dev server uses Python 3.

```bash
git clone https://github.com/mari0-th/InfotronX-Horizon.git
cd InfotronX-Horizon
npm run dev        # http://localhost:4173
npm test           # the full test suite
npm run build      # static site in dist/, ready for any static host
```

There is no compiler and no bundler: the app is native ES modules. Three.js comes from a CDN through an import map.

## Under the hood

| Area | Primary files |
| --- | --- |
| Shell, panels, steps | `index.html`, `src/main.js`, `src/workflow-steps.js`, `src/design.css` |
| 3D planet | `src/map-3d.js`, `src/planet-mesh.js`, `src/mars-sites.js`, `src/deep-space.js` |
| 2D map | `src/map.js`, `src/constants.js` |
| Routing and terrain | `src/terrain.js`, `src/route-decision.js`, `src/dstar-lite.js`, `src/pareto-routes.js` |
| Analyze and benchmark | `src/analyze-dashboard.js`, `src/benchmark.js`, `src/companions.js` |
| Planning Lab | `src/lab.js` (the models), `src/lab-panel.js` (the window) |
| Science tools | `src/science-workbench.js`, `src/advanced-math.js`, `src/terrain-topology.js` |
| Missions, accounts, sharing | `src/store.js`, `src/account.js`, `src/share.js` |
| Height data | `assets/jezero_ctx_trn_dem_20m_clip.json` |
| Tests | `tests/` (maths, benchmark, Lab and more), `.github/workflows/verify.yml` |

The tests re-plan the benchmark routes on the bundled height model, check every worked example on this page and run every Lab card on its own example, so the documentation cannot drift from the program.

## Contribute

Short branches and Pull Requests into `main`, with `npm test` passing. See [CONTRIBUTING](CONTRIBUTING.md), [Engineering standards](docs/DEVELOPMENT_STANDARDS.md) and [Current state](docs/PROJECT_STATE.md). Sharper zoom in 2D and 3D, real planet textures and a Content-Security-Policy are the open items with the most impact.

## Credits and honesty

**InfotronX Horizon** is an independent team. The clean Git repository is a publication reset, **not evidence that all components were written during the hackathon**; see [Submission readiness](docs/SUBMISSION_READINESS.md). The software was built with AI assistance (Claude by Anthropic) for code and documentation; tools and contributors are in [SUBMISSION.md](SUBMISSION.md) and [CREDITS.md](CREDITS.md). The project's software licence does not replace third-party data and imagery licences.

## Legal

- Licence: [MIT](LICENSE). Copyright (c) 2026 InfotronX Horizon and Mars Atlas contributors.
- [Terms of Use](TERMS.md) · [Privacy Policy](PRIVACY.md) · [Cookies and local storage](COOKIES.md) · [Credits](CREDITS.md). The same pages are inside the app at `legal.html`.
- Versions are tagged `vMAJOR.MINOR.PATCH`; see the [Changelog](CHANGELOG.md) and the [release runbook](docs/RELEASE_RUNBOOK.md).

<div align="center">

*Made for people who want to understand a place before they go there.*

</div>
