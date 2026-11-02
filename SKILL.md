---
name: geoskill-hyperspectral-unmixing
description: 'VCA/N-FINDR端元提取+nnls线性解混丰度反演'
---

# 高光谱端元提取与解混 | Hyperspectral Endmember Extraction & Unmixing

Extracts pure endmember spectra from hyperspectral imagery and inverts per-pixel abundances. Two classic endmember-extraction algorithms are supported:

- **VCA** (Vertex Component Analysis): iteratively seeks the projection direction with maximum variance in PCA-projected space and takes the extreme pixels as endmembers;
- **N-FINDR** (simplified): a greedy iterative search for the pixel combination that maximizes the simplex volume.

Linear unmixing uses FCLSU: for each pixel, `scipy.optimize.nnls` solves for non-negative least-squares abundances, which are then normalized to sum to 1 (abundance sum-to-one constraint); a residual map is also output to evaluate fit quality. Suitable for mixed-pixel decomposition, mineral/vegetation abundance mapping, and sub-pixel analysis.

## Dependencies / 依赖

```bash
pip install numpy rasterio scipy
```

## Usage / 使用方法

### Basic Usage (Synthetic Data, Offline)

```bash
python geoskill-hyperspectral-unmixing.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./out
```

### Example 1: VCA with 3 Endmembers

```bash
python geoskill-hyperspectral-unmixing.py \
    --bbox 116.0 39.0 117.0 40.0 \
    --synthetic --n-endmembers 3 --method vca \
    --output-dir ./vca_3em
```

### Example 2: N-FINDR Method Comparison

```bash
python geoskill-hyperspectral-unmixing.py \
    --bbox 121.0 31.0 122.0 32.0 \
    --synthetic --n-endmembers 4 --method nfindr \
    --output-dir ./nfindr_4em
```

### Example 3: More Bands

```bash
python geoskill-hyperspectral-unmixing.py \
    --bbox 116.0 39.0 117.0 40.0 \
    --synthetic --n-bands 40 --n-endmembers 3 \
    --output-dir ./vca_40bands
```

### Example 4: Real Hyperspectral GeoTIFF Input

```bash
python geoskill-hyperspectral-unmixing.py \
    --input cuprite_subset.tif --n-endmembers 5 --method vca \
    --output-dir ./real_vca
```

## Output / 输出

| File | Format | Description |
|---|---|---|
| `abundances.tif` | GeoTIFF (float32, one band per endmember) | Normalized abundance map [0,1], EPSG:4326 |
| `residual.tif` | GeoTIFF (float32) | Linear unmixing residual (RMSE) map |
| `endmembers.json` | JSON | Endmember spectra, algorithm, matching diagnostics |
| `output-manifest.json` | JSON | Run manifest (input/output/QA/software versions) |

## Data Source / 数据源 / Source

- **Real mode**: local hyperspectral GeoTIFF
- **Synthetic mode**: defines endmember spectra locally and generates pixels by linear mixing with random abundances plus noise; no external data source

## Privacy / 隐私声明 / Privacy

- Runs fully offline by default and makes no network requests
- `--synthetic` mode reads no external data
- All computation is done locally; user data is never uploaded

## License / License

MIT

---

> 中文完整版：需要时再读 [references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)，不要默认全量加载。
