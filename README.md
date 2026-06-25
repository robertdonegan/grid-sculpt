# Grid Sculpt

A self-contained, single-file browser tool for visualising and manually
sculpting computational mesh resolution over a real-world basemap. Built for
hydraulic/topographic modelling workflows (river reaches, building clusters,
DTM/DEM-aware mesh refinement) with mesh export aimed at tools like TUFLOW or
HEC-RAS in mind.

No build step, no dependencies, no install. It's one HTML file — open it in a
browser and it runs.

## Quick start

- **Live demo:** `https://yourname.github.io/grid-sculpt/` *(replace with your Pages URL)*
- **Or run locally:** download `index.html` and open it directly in any modern browser.

There's no server-side component, nothing to build, nothing to `npm install`.

## What it does

- Quadtree-based mesh with 2:1 balance enforcement, brush-based refine/coarsen,
  flood fill, falloff-based brushing, and Alt-key inversion.
- Geo-anchored to real lon/lat — the mesh pans and zooms in lockstep with the
  basemap underneath it.
- Live basemap imagery (street map or satellite), with search-by-place
  geocoding.
- **Shape tools** — Square / Freeform / Line / Breakline — for applying a
  target resolution to a hand-drawn area instead of brushing cell by cell.
- **Random scheme generator** — scatters a meandering river reach and random
  building clusters at higher resolution, as a quick stand-in for what
  pre-existing DTM/DEM topology might dictate.
- **Import real building footprints** from GeoJSON or raw ESRI Shapefile
  (`.shp`), with optional per-class target depth (e.g. buildings finer than
  roads) driven by a GeoJSON property.
- **DEM-aware auto-refinement** from an Esri ASCII Grid (`.asc`) — resolution
  is set automatically from local terrain slope, so steep ground gets refined
  and flat ground gets coarsened.
- **Grid rotation** — rotate the mesh to align with a road, river, or site
  boundary, with a configurable snap interval.
- **Mesh export** to GeoJSON, geo-referenced, with each cell tagged by its
  resolution depth.

## Controls

| Action | Input |
|---|---|
| Sculpt (refine/coarsen) | left-click drag |
| Invert brush mode | hold `Alt` |
| Brush radius | scroll wheel |
| Pan the basemap | right-click drag |
| Zoom the basemap | `Shift` + scroll |
| Undo | `Ctrl`/`Cmd` + `Z` |
| Sculpt (touch) | 1 finger drag |
| Pan + zoom basemap (touch) | 2 finger drag / pinch |

## Importing your own data

Both import paths assume **WGS84 (lon/lat, EPSG:4326)** coordinates. If your
source data is in a projected CRS (e.g. British National Grid, EPSG:27700),
reproject it first — in QGIS: right-click the layer → Export → Save Features
As → set CRS to `EPSG:4326`. Loading projected coordinates without
reprojecting will place features in the wrong location without warning.

- **Footprints:** `.geojson`/`.json` (Polygon/MultiPolygon features) or raw
  `.shp` (Polygon shape type only — `.dbf`/`.shx`/`.prj` are not read).
  Optional per-class depth: set a property field name and a JSON map like
  `{"building": 6, "road": 3}`.
- **DEM:** Esri ASCII Grid (`.asc`), with `xllcorner`/`yllcorner`/`cellsize`
  in decimal degrees.

## Known limitations

- **Live imagery requires a normal browser context.** If this is opened
  inside a sandboxed preview (e.g. an AI chat artifact), the basemap tiles
  and geocoding will be blocked by that sandbox's network policy — this is a
  property of the sandbox, not a bug in this code. GitHub Pages, or any other
  normal static hosting, doesn't have that restriction.
- Shapefile import reads geometry only — attribute data (`.dbf`) isn't
  parsed, so per-class depth currently only works via GeoJSON properties.
- Undo is a single step, not a full history stack.
- Resizing the browser window currently rebuilds the grid from scratch,
  discarding any sculpted detail. Export your mesh first if you need to keep
  it across a resize.

## Feedback / contributing

This is an early-stage tool for testing and feedback — issues and PRs welcome.
If you hit a bug, the browser console (F12 → Console) almost always has a
more specific error message than what's visible on screen; including that in
a bug report helps a lot.

## License

*(add your preferred license here — MIT is a reasonable default for a tool
like this if you don't already have one in mind)*
