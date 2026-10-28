# IGNIS Burned-Area Segmentation

Lightweight segmentation companion to [`Nabidnur/ignis-fire-calendar`](https://huggingface.co/datasets/Nabidnur/ignis-fire-calendar) (private dataset).

## What it does
Segments the day's active-fire detections into coherent fire **clusters** and
produces convex hulls + morphology stats, so one map red dot cloud becomes
named, measurable fire perimeters.

## Pipeline
1. DBSCAN-style density clustering over the day's detections (eps adaptive to
   sensor: 1 km MODIS vs 375 m VIIRS).
2. Convex hull + area/elongation/night-share features per cluster.
3. Classification head assigns cluster class (mirrors the fire-type model).
4. Hulls render as colour-coded polygons on both the WebGL and GPU-free maps.

## Size & runtime
Pure TypeScript, **0 dependencies**, runs in < 40 ms for 25 000 detections
(`src/lib/ignis/segmentation.ts` + `CLASS_STYLE`). The heavier learned
variant (U-Net over GIBS 7-2-1 composites, 3-band 512² tiles) trains in the
HF dataset pipeline and exports to the same hull format.

## Outputs per cluster
`n`, `frpSum`, `frpMean`, `spreadKm`, `areaKm2`, `elongation`, `nightPct`,
`modelCls`, `modelClsPct`, hull ring.

## License & release
Private during judging; Apache-2.0 re-release planned post-competition.
No credentials in this repository.
