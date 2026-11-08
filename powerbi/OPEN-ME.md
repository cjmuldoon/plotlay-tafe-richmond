# Opening the Power BI report

`TafeRichmondBuildings.pbip` is a Power BI Project: the report (`TafeRichmondBuildings.Report`, PBIR format) and the semantic model (`TafeRichmondBuildings.SemanticModel`, TMDL) saved as text, so the repo can show every change. Power BI Desktop opens it like a `.pbix`.

## What you need

- Power BI Desktop for Windows, a recent version (2026). If Desktop is older than about mid 2025, turn on **File, Options and settings, Options, Preview features, Power BI Project (.pbip) save option** and **Store reports using enhanced metadata format (PBIR)**, then restart Desktop.
- The repo folder on the Windows machine, wherever you cloned or unzipped it.

## Open it

1. In Windows, open File Explorer and go to the `powerbi` folder, wherever you cloned or unzipped the repo.
2. Double-click `TafeRichmondBuildings.pbip`, or in Desktop: **File, Open report, Browse reports**, file type *Power BI project files (\*.pbip)*.
3. The visuals are empty the first time. Select **Refresh** on the Home ribbon.
   - By default the data is read from the repo's public copy on GitHub (`DataFolder` is `https://raw.githubusercontent.com/cjmuldoon/plotlay-tafe-richmond/main/data/`). If Desktop asks how to connect, choose **Anonymous** and **Connect**.
   - To read the files on disk instead: **Home, Transform data, Edit parameters**, set `DataFolder` to the full path of the repo's `data` folder **ending with a backslash**, then **OK** and **Apply changes**.
   - If Desktop warns about privacy levels, choose **Ignore privacy levels** for this file (all sources are public open data) or set them to Public.
4. Check the numbers (below).

## Check the numbers after Refresh

After **Refresh** (there are no slicers, and nothing on these pages changes with time):

| Page | Card | Should read |
|---|---|---|
| Drawn out | Buildings | 38 |
| Drawn out | In no open data | 5 |
| Drawn out | Microsoft offset, median m | 2.23 |

These are the README's figures. The table lists all 38 buildings (18,109 m² of footprint in all); the offset chart on **Outline sources** shows the 31 buildings started from a Microsoft footprint, largest offset first.

The sample pages (not about any real building) read: Assets 372, High or extreme 70, Past expected life 106; Open 33, Overdue 9, Raised in 12 months 1,010; Planned maintenance on time 88.1%, Safety measures overdue 8, Capital works backlog 3,054,000.

## The tables

`Buildings` has one row per building (the key the shapes carry): where its outline came from, how accurate it is likely to be, its footprint as drawn, and how far Microsoft's footprint was off. `Site` is one row: the campus as drawn along its fences and roads, with the buildings rolled up in Plotlay.

## The sample data pages

Three pages read the tables in `data/sample/`: **Sample: risk** (the risk matrix, the assets by risk, each building's assets at high or extreme risk), **Sample: work orders** (open by priority and building, raised each month, reactive against planned) and **Sample: compliance** (planned maintenance on time, essential safety measures by status, the capital works backlog). The tables are `SampleSummary`, `SampleAssets`, `SampleRisk`, `SampleWorkOrders`, `SamplePM` and `SampleESM`; their measures are in the Sample data folder. They are sample data, not about TAFE NSW Richmond or any real building, and each page carries a Sample data tag. No other page or measure reads them.

## The maps

- **Shape Map** (Drawn out, and Sample: risk). The custom map, `data/geometry/campus_buildings.topojson`, is saved inside the report (`TafeRichmondBuildings.Report/StaticResources/RegisteredResources/campus_buildings.topojson`, a copy that keeps only the building key), so the 38 buildings draw on open: by outline source on the first page, by sample assets at high or extreme risk on the risk page. The Shape Map has no base map of its own, so the file also carries the context, drawn underneath in a pale ground tone: the NSW Cadastre lots round the campus (the gaps between them are the roads), the suburb boundaries (ABS) and the campus as a thin outline. Loading the plain file from `data/` by hand (below) gives the buildings without that context.
- If a map shows the United States instead (an older Desktop can ignore a saved map): select it, then **Format visual** (the paintbrush), **Visual**, **Map settings**, **Map type: Custom map**, **Add map type** (or **Browse**), and choose `..\data\geometry\campus_buildings.topojson`. Under **View map keys** the keys B01 to B38 should match. Projection: Mercator. If the Shape Map is not in the Visualizations pane, turn it on in **File, Options and settings, Options, Preview features, Shape map visual** and restart Desktop.
- If a Shape Map comes up empty after opening, drag the fields back: Location `Buildings[key]`, Legend `Buildings[outline_source]` (first page) or Color saturation `[Sample high or extreme]` (risk page).
- Optional, if you add an **Azure Maps** visual (for real roads under the buildings): add a reference layer from `..\data\geometry\campus_buildings.geojson`, join property `key`, map style **Grayscale (light)** or **Blank**. Do not publish satellite basemap screenshots.

## Save a .pbix too

After the refresh: **File, Save as**, type **Power BI file (\*.pbix)**, save as `powerbi\TafeRichmondBuildings.pbix`. It includes the data.

## Publish (only if you choose to)

1. **Home, Publish**, sign in, pick a workspace.
2. In the service, the semantic model's **Settings, Data source credentials**: the GitHub address uses **Anonymous**. Scheduled refresh then works with no gateway.
3. **Publish to web** makes a public embed link; anyone with the link can see the report. Only if you intend it to be public.

## Shot list for the website and the video

Capture at 1920 × 1080 (Windows display scaling 100%), Plotlay theme on. Snipping Tool (Win+Shift+S) for stills, Win+Alt+R or Clipchamp for clips. Save to `powerbi/Screenshots/`.

| # | Shot | How | Length |
|---|---|---|---|
| 1 | Drawn out | The Shape Map coloured by outline source, the cards and the table | still |
| 2 | Pick a building | Click B01 in the table: the map and cards follow, its floor share 75.9% | 3 s clip |
| 5 | Sample: risk | The risk matrix and the assets by risk. Keep the Sample data tag and the footer in shot | still |
| 5a | Sample: work orders | Click P3 Routine: the buildings and months follow | 3 s clip |
| 5b | Sample: compliance | Planned maintenance on time by month and the safety measures by building | still |
| 6 | Model view | The tables, Buildings in the middle, the sample tables related to it by building key | still |
| 7 | Power Query | **Transform data**: the `DataFolder` parameter and the steps of one sample table | still |

Keep the footer with the attribution visible in every shot.

## What's in the folder

- `TafeRichmondBuildings.pbip`: open this.
- `TafeRichmondBuildings.SemanticModel/definition/`: `tables/*.tmdl` (columns, measures, Power Query), `relationships.tmdl`, `expressions.tmdl` (the `DataFolder` parameter and a small `GetFile` function).
- `TafeRichmondBuildings.Report/definition/`: one folder per page and visual (PBIR), `report.json` (the Plotlay theme registered).
- Pages: Drawn out, Outline sources, Sample: risk, Sample: work orders, Sample: compliance.
