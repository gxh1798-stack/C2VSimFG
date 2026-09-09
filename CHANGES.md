# C2VSimFG — change register

Base: **C2VSimFG 1.5** (DWR Version 1.5), tag `C2VSimFG_1.5`. Each row below is one commit / one tag / one
suggested change. Status: *proposed* until reviewed; reviewers mark *accepted* / *rejected*.

| tag | Log.txt version | change (one line) | files touched | depends on | status |
|---|---|---|---|---|---|
| `C2VSimFG_1.5` | C2VSimFG 1.5 | DWR Version 1.5 (unmodified base) | — | — | base |
| `C2VSimFG_1.5+cb1` | C2VSimFG 1.5+cb1 | Race condition: delivery adjustment - KOPTDV 11 to 10 (adjust GW pumping only; SW diversions are observed data) | Simulation/C2VSimFG.in (KOPTDV) | — | proposed |
| `C2VSimFG_1.5+cb2` | C2VSimFG 1.5+cb2 | Race condition: multiple stream nodes sharing a GW node - CSTRM=0 at 83 confluence reach-end nodes (only the receiving node exchanges with the aquifer) | Simulation/Streams/C2VSimFG_Streams.dat (Stream Bed Parameters, CSTRM) | — | proposed |
| `C2VSimFG_1.5+cb3` | C2VSimFG 1.5+cb3 | Erroneous diversion sources: Kings River spreading diversions 261/263/265 (FID/CID/ALTA spreading) IRDV 0 to 647 - draw from the Kings River instead of from nowhere (double-count) | Simulation/Streams/C2VSimFG_DiversionSpec.DAT (IRDV, rows 261/263/265) | — | proposed |
| `C2VSimFG_1.5+cb4` | C2VSimFG 1.5+cb4 | Kings River Nov-Feb diversions: cap FID/CID/ALTA crop (AG) diversions at crop demand and move the surplus to the paired spreading diversions (Diversions.DAT cols 260-265, 237 month-values) | Simulation/Streams/C2VSimFG_Diversions.DAT (cols 260/261, 262/263, 264/265, Nov-Feb rows) | cb3 (spreading diversions must draw from node 647) | proposed |
| `C2VSimFG_1.5+cb5` | C2VSimFG 1.5+cb5 | Upgrade to IWFM 2025.0.1747: root-zone file converted 4.11 to 4.12 (adds per-land-use surface-flow destinations via new SurfFlowDest file; routing unchanged), executables and version banners updated | Simulation/RootZone/C2VSimFG_RootZone.dat (4.11 to 4.12); NEW Simulation/RootZone/C2VSimFG_SurfFlowDest.dat; IWFM version banner in 36 input files; 4 .bat files; README.md | — | proposed |
| `C2VSimFG_1.5+cb6` | C2VSimFG 1.5+cb6 | WWTP discharge destinations: route urban (M&I) return flow of the 26 southern evaporation-pond city clusters (6,071 elements; Fresno-Clovis, Visalia, Hanford, Merced, Madera, Porterville, Corcoran, Delano) out of the model instead of to stream nodes | Simulation/RootZone/C2VSimFG_RootZone.dat (ICDSTURBIN/ICDSTURBOUT of 6,059 elements to SurfFlowDest column 1 = (0,0)) | cb5 (per-land-use destinations) | proposed |
| `C2VSimFG_1.5+cb7` | C2VSimFG 1.5+cb7 | Irrigation return flows: crop return-flow (and reuse) fraction column 3 ramped from 5% (flood era, through WY1990) in equal annual steps to 1% by WY2000 (sprinkler/drip conversion) - static single row replaced by a 576-row monthly series | Simulation/RootZone/C2VSimFG_ReturnFlowFrac.dat, C2VSimFG_ReuseFrac.dat (data block: 1 row -> 576 rows, col 3) | — | proposed |
| `C2VSimFG_1.5+cb8` | C2VSimFG 1.5+cb8 | Crop evapotranspiration reshaped to the OpenET-calibrated seasonal profile (420 ag ETc columns scaled per crop x month to the C2VSimCG v2025 calibrated profile; 99% of OpenET valley-wide) | Simulation/C2VSimFG_ET.dat (1,212 monthly data rows; header and structure unchanged) | — | proposed |
| `C2VSimFG_1.5+cb9` | C2VSimFG 1.5+cb9 | Stream rating tables derived from observed stage-flow data: 3,422 of 4,634 nodes re-rated from USGS/CDEC/NOAA observations (Preprocessor input; regenerated PreprocessorOut.bin included) | Preprocessor/C2VSimFG_StreamsSpec.dat (rating-table block: 3,422 nodes x 9 rows); Simulation/C2VSimFG_PreprocessorOut.bin (regenerated) | — | proposed |
| `C2VSimFG_1.5+cb10` | C2VSimFG 1.5+cb10 | Monthly time-step demand correction: non-ponded crop TargetSM scaled per subregion (21 factors 0.85-1.21) to reproduce daily-run water use (20 of 21 subregions within 1%; pumping +1.0% to -0.1%; streamflows within 0.4% at major gages) | Simulation/RootZone/C2VSimFG_NonPondedCrop_TargetSM.dat | cb1-cb9 (factors calibrated against the cumulative model) | proposed |
| `C2VSimFG_1.5+cb11` | C2VSimFG 1.5+cb11 | Dry Creek rim inflow corrected x2.73: column 29 (stream node 2442, reach 49) held the USGS 11329500 record divided by the gauge-to-boundary drainage-area ratio; the factor nets out the 100 sq mi already covered by small watersheds 885-888 so the reach carries the full gaged 324 sq mi (gage fit 0.32 to 0.70 of observed, r 0.91 to 0.97) | Simulation/Streams/C2VSimFG_StreamInflow.dat | cb10 | proposed |

## How to review one change

```sh
git tag -n                              # list all changes with their one-line messages
git diff C2VSimFG_1.5+cb2 C2VSimFG_1.5+cb3 --stat   # files touched by change 3
git diff C2VSimFG_1.5+cb2 C2VSimFG_1.5+cb3          # the exact edit
git show C2VSimFG_1.5+cb3                     # commit message (issue / resolution / result) + diff
```

## How to accept only some changes

Start from the base and cherry-pick the accepted tags in order:

```sh
git checkout -b accepted C2VSimFG_1.5
git cherry-pick C2VSimFG_1.5+cb1 C2VSimFG_1.5+cb3 C2VSimFG_1.5+cb4
```

Model files are kept independent between changes (each change edits its own files/sections), so
picks apply cleanly. `Log.txt` and `CHANGES.md` are appended by every change and will report a
trivial conflict when a change is skipped — keep both sides' rows and `git cherry-pick --continue`.
Where a change genuinely builds on another, the *depends on* column says so; pick the dependency first.
