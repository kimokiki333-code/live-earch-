<div align="center">

# ⬡ ARGUS

### Global Monitoring & Intelligence

**The next generation of OSIRIS — focused, clean, and framing-free.**

[![Next.js](https://img.shields.io/badge/Next.js-16-black?style=for-the-badge&logo=next.js)](https://nextjs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?style=for-the-badge&logo=typescript&logoColor=white)](https://typescriptlang.org)
[![MapLibre](https://img.shields.io/badge/MapLibre_GL-GPU_Rendered-396CB2?style=for-the-badge)](https://maplibre.org)
[![License](https://img.shields.io/badge/License-MIT-D4AF37?style=for-the-badge)](LICENSE)

*A live global picture: 3D globe, 2D map, satellite imagery and night mode over one GPU-accelerated
map, with flight, maritime, satellite, camera and cyber feeds on top.*

</div>

---

## What this is

`ARGUS GMI` is the successor to the OSIRIS dashboard in this repository (which itself is the
`live-earch-` monitoring build). The map is the product: **3D · 2D · SAT · NIGHT** — four ways to
look at the same live world — and everything else is a layer or an instrument on top of it.

The interface was stripped of anything that made it read like a surveillance-camera feed or a
playing recording, and of the tools that were not about watching the world map.

### The four map modes

| Mode | What it is |
|------|------------|
| **3D** | MapLibre globe, tilted, with optional 3D terrain and city buildings |
| **2D** | Flat Mercator projection of the same data, for measuring and reading |
| **NIGHT** | Dark vector basemap — the default, high-contrast operating view |
| **SAT** | ArcGIS World Imagery raster under every layer |

One segmented control switches projection and basemap independently, so any combination works.

### What stays

- **Live map layers** — aviation (commercial / private / jets / military), maritime ports,
  chokepoints and vessels, the full satellite catalogue in 3D orbit, CCTV cameras with live
  preview tiles, live news broadcast pins, conflict zones, GDELT/GDACS incidents, nuclear
  reference sites, submarine cables, day/night terminator, 3D terrain and buildings.
- **Left layer rail** — grouped flyout toggles with live entity counts per layer.
- **Right tool strip** — RECON, SPACE (ISS downlink), ALERTS, ROUTE, SEARCH.
- **Panels** — Live Alerts, satellite card, camera viewer, region dossier, flight watch,
  view presets, style studio, keyboard shortcuts.
- **RECON toolkit** — the OSINT utilities (IP lookup, network sweep, geolocation) that arrived
  with the `live-earch-` scanner build.
- **Routing and search** — turn-by-turn directions and place/coordinate search.

### What was removed in this pass

| Removed | Where it lived |
|---------|----------------|
| **Markets** panel and tab | right tool strip, mobile nav, `/api/markets` polling |
| **ArcGIS Intel** importer | right tool strip, `/api/arcgis` layer import |
| **Draw / Measure** toolbar and HUD | right tool strip, AOI polygons, tripwires |
| **World Remote** (Bluetooth) | right tool strip, mobile nav |
| **Natural Hazards** layer group | left layer rail (earthquakes / fires / severe weather) |
| **Camera / recording framing** | CRT scanlines, edge vignette, corner brackets, splash grid + bezel |

Everything else is kept and wired to the same data sources as before.

---

## Quick start

```bash
npm install
npm run dev
```

Open <http://localhost:3000>.

```bash
npm run build   # production build
npm start       # serve the build
npm test        # offline unit tests
```

### Docker

```bash
docker compose up -d
# or
docker pull ghcr.io/simplifaisoul/osiris:latest
docker run -d -p 3000:3000 --env-file .env ghcr.io/simplifaisoul/osiris:latest
```

See **[DOCKER.md](DOCKER.md)** for the full self-hosting guide.

### Environment

Everything core is keyless. Optional keys only raise rate limits or unlock extra feeds:

```env
SCANNER_URL=      # RECON backend
SCANNER_KEY=
FIRMS_API_KEY=
OPENSKY_CLIENT_ID=
OPENSKY_CLIENT_SECRET=
N2YO_API_KEY=
AIS_API_KEY=
```

---

## Architecture

```
┌──────────────────────────────────────────────────┐
│                    ARGUS CLIENT                   │
│  ┌────────────┐ ┌──────────┐ ┌────────────────┐  │
│  │ MapLibre   │ │ HUD      │ │ RECON toolkit  │  │
│  │ 3D/2D/SAT  │ │ panels   │ │ OSINT utilities│  │
│  │ + NIGHT    │ │ layers   │ │                │  │
│  └────────────┘ └──────────┘ └────────────────┘  │
├──────────────────────────────────────────────────┤
│                NEXT.JS API ROUTES                 │
│  /api/flights /api/satellites /api/cctv          │
│  /api/maritime /api/news /api/conflicts          │
│  /api/osint/* /api/entity/* /api/ai/*            │
├──────────────────────────────────────────────────┤
│               EXTERNAL DATA SOURCES               │
│  OpenSky · USGS · NASA · NOAA · TfL · NVD        │
│  GDACS · EONET · FIRMS · N2YO · RSS · t.me       │
└──────────────────────────────────────────────────┘
```

| Layer | Technology |
|-------|-----------|
| Framework | Next.js 16 (App Router, Turbopack) |
| Language | TypeScript 5 |
| Map engine | MapLibre GL JS (WebGL) |
| Animation | Framer Motion |
| Icons | Lucide React |
| Styling | Custom CSS design system |

---

## Merging another project in

This branch is the merge point. The interface is now deliberately narrow — one map, four modes,
one layer rail, one tool strip — so a second project's features have an obvious place to land:

1. **New map layers** → add the fetch in `src/app/page.tsx`, the layer definition in
   `src/components/LayerPanel.tsx`, and the styling in `src/components/OsirisMap.tsx`.
2. **New instruments / panels** → add a button to the right tool strip in `src/app/page.tsx`
   (desktop) and a tab in the mobile nav, then mount the panel beside it.
3. **New backend capabilities** → drop the route under `src/api/<feature>/route.ts`.
4. **Branding** → the name lives in `src/app/layout.tsx` (metadata), `src/app/page.tsx`
   (header and splash) and `package.json`.

Because the four modes, the layer rail and the tool strip are the only furniture on the screen,
anything added from another codebase either becomes a layer, a tool, or a page — it does not need
its own chrome.

---

## Acknowledgements

Public webcams in the camera layer are catalogued from [bekijkhet.nu](https://www.bekijkhet.nu/),
which has curated them by hand since 2012. Basemap © CARTO / OpenStreetMap contributors;
satellite imagery © Esri World Imagery. ARGUS links every camera straight through to the
operator who runs it.

---

## License

MIT — see [LICENSE](LICENSE).

<div align="center">

**Built on the OSIRIS project by [simplifaisoul](https://github.com/simplifaisoul)** · ARGUS GMI

</div>
