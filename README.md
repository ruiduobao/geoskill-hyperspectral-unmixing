# Hyperspectral Endmember Extraction and Unmixing (geoskill-hyperspectral-unmixing)

> VCA/N-FINDR endmember extraction + nnls linear unmixing abundance inversion

---

## 1. Overview

Extracts pure endmember spectra from hyperspectral imagery and inverts per-pixel abundances. Two classic endmember extraction algorithms are supported: - **VCA** (Vertex Component Analysis): iteratively searches for the projection direction with maximum variance in the PCA-projected space, taking extreme pixels as endmembers; - **N-FINDR** (simplified): greedy iterative search for the pixel combination that maximizes the simplex volume. Linear unmixing uses FCLSU: for each pixel, `scipy.optimize.nnls` computes non-negative least squares abundances, normalized to sum to 1 (abundance sum-to-one constraint), and a residual map is output to assess fit quality. Suitable for mixed-pixel decomposition, mineral/vegetation abundance mapping, and sub-pixel analysis.

## 3. Quick Start

```bash
pip install -r requirements.txt
python geoskill-hyperspectral-unmixing.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

## 4. CLI Parameters

Run `python <skill>.py --help` for the full list. Common parameters:

| Parameter | Type | Description |
|---|---|---|
| `--bbox` | `float[4]` | WGS84 bounding box `min_lon min_lat max_lon max_lat` |
| `--input` | `path` | Local input file (GeoJSON/GeoTIFF/etc.) |
| `--output-dir` | `path` | Output directory (default `./output`) |
| `--synthetic` | `flag` | Use synthetic data instead of real input |
| `--quiet` | `flag` | Suppress non-essential stdout |

## 5. Input / Output

| File | Format | Description |
|---|---|---|
| `abundances.tif` | GeoTIFF (float32, one band per endmember) | Normalized abundance map [0,1], EPSG:4326 |
| `residual.tif` | GeoTIFF (float32) | Linear unmixing residual (RMSE) map |
| `endmembers.json` | JSON | Endmember spectra, algorithm, matching diagnostics |
| `output-manifest.json` | JSON | Run manifest (input/output/QA/software versions) |


## 6. Technical Principle

(see SKILL.md for details)

## 8. License

MIT License. See [`LICENSE`](./LICENSE) for full text.

---

> 中文完整版：[references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)
