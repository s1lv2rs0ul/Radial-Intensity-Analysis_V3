# Radial Intensity Analysis

Radial intensity profiling of fluorescence microscopy images of micropatterned colonies. Given multichannel TIFFs, the pipeline segments nuclei, finds the colony center, and computes per-channel intensity as a function of distance from the center.

## What it does

For each input TIFF the pipeline:

1. Max-projects the Z-stack to 2D
2. Smooths and contrast-enhances each channel
3. Segments nuclei in the DAPI channel using StarDist (`2D_versatile_fluo`)
4. Computes the colony center as the center of mass of the nuclear mask
5. Bins pixel intensities by distance from the center to produce a radial profile per channel
6. Saves per-image plots (max projection, segmentation, heatmaps, profiles), a CSV, and a PowerPoint summary

Two notebooks are included:

- **`Normalization_code_V3.ipynb`** — normalizes each profile to its own max. Use for comparing the **shape** of the gradient across colonies.
- **`Non-Normalization_Code_V3.ipynb`** — keeps absolute intensity. Use for comparing **magnitude** across colonies.

## Input

A folder of 4D TIFFs in (Z, channel, Y, X) order. The current script assumes 3 channels:

| Channel | Marker |
|---------|--------|
| C0 | ZO-1 |
| C1 | DAPI |
| C2 | SMAD2 |

Pixel scale is set to `0.5 µm/px` at the top of each notebook — change it for your microscope.

## Output

For an input folder `images/` the pipeline creates:

```
images/
├── radial_outputs/      # <image>_radial.csv per image
├── plots/<image>/       # per-image PNGs
├── summary/             # combined CSV + per-channel cross-image plots
└── ppt_reports/         # batch_report.pptx
```

## Install

Tested on macOS with Python 3.10. With conda (recommended):

```bash
conda env create -f environment.yml
conda activate radial
```

Or with pip into your own environment:

```bash
pip install -r requirements.txt
```

StarDist will download its pretrained model on first run.

## Run

1. Open one of the notebooks in Jupyter.
2. Edit the `base_dir` variable near the bottom to point at your folder of TIFFs.
3. Run all cells.

## Citation

If you use this in published work, please cite this repository.

## License

MIT — see [LICENSE](LICENSE).
