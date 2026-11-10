# CellGPS

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Upload Python Package](https://github.com/Moldia/CellGPS/actions/workflows/python-publish.yml/badge.svg)](https://github.com/Moldia/CellGPS/actions/workflows/python-publish.yml)

`CellGPS` is the Python package and reference implementation for Cophenetic Spatial Topology Embedding (COSTE), a spatial topology analysis framework for spatial omics data.

This repository is maintained as both the installable Python package and the code companion for the CellGPS/COSTE bioRxiv preprint.

CellGPS is maintained in the Moldia organization. This public repository is the
canonical source for development and the scverse ecosystem application. See
[the migration guide](docs/project/MIGRATION.md) when moving from the historical
Cell-GPS distribution or the personal repository.

## Implementations and availability

[Moldia/CellGPS](https://github.com/Moldia/CellGPS) is the primary, full Python implementation. It contains all CellGPS functions and the complete reproduction code for the bioRxiv preprint. The [Moldia/cellgpsr](https://github.com/Moldia/cellgpsr) R package and the Windows files distributed in [Zenodo record 19482685](https://zenodo.org/records/19482685) provide only the core functionality; they do not include the full Python function set or the complete preprint reproduction code.

## Preprint and manuscript code

CellGPS/COSTE is described in the associated bioRxiv preprint:

> Mengping Long, Taobo Hu, Alexandros Sountoulidis, Christos Samakovlis, and Mats Nilsson (2026). *Cophenetic Spatial Topology Embedding reveals multiscale tissue architecture in spatial omics*. bioRxiv preprint. [doi:10.64898/2026.05.26.727847](https://doi.org/10.64898/2026.05.26.727847). [Version 1 on bioRxiv](https://www.biorxiv.org/content/10.64898/2026.05.26.727847v1).

If you use the Python package, the Windows executable, the R companion package, or the manuscript figure/table code, please cite this preprint.

The code used to generate the preprint figures and supplementary tables is organized in [`CellGPS manuscript/`](https://github.com/Moldia/CellGPS/tree/main/CellGPS%20manuscript):

- [`main_figures/`](https://github.com/Moldia/CellGPS/tree/main/CellGPS%20manuscript/main_figures): notebooks for main figure analyses.
- [`supplementary_figures/`](https://github.com/Moldia/CellGPS/tree/main/CellGPS%20manuscript/supplementary_figures): notebooks for supplementary figure analyses.
- [`supplementary_tables/`](https://github.com/Moldia/CellGPS/tree/main/CellGPS%20manuscript/supplementary_tables): notebooks for supplementary table analyses.

The notebooks are intentionally output-free and preserve the original manuscript data paths where those paths were required for reproduction. A detailed mapping from manuscript results to source code is available in [`docs/cellgps_science_manuscript_code_inventory.md`](https://github.com/Moldia/CellGPS/blob/main/docs/cellgps_science_manuscript_code_inventory.md).

## COSTE extension manuscripts

Beyond the main CellGPS/COSTE preprint above, the [`manuscripts/`](https://github.com/Moldia/CellGPS/tree/main/manuscripts) directory collects the analysis and figure-generation code for follow-up manuscripts that build on the COSTE method. Each subfolder is a self-contained study with its own README describing the dataset, scripts, and reproduction steps:

- [`manuscripts/atera_urna/`](https://github.com/Moldia/CellGPS/tree/main/manuscripts/atera_urna): Atera uRNA-COSTE study — segmentation-bias check using unassigned RNA (uRNA, transcripts not assigned to a segmented cell) and uRNA-COSTE concordance analysis.
- [`manuscripts/segmentation_free_transcript_domains/`](https://github.com/Moldia/CellGPS/tree/main/manuscripts/segmentation_free_transcript_domains): segmentation-free spatial domain discovery from transcript points alone.

For details on any extension manuscript, read the README inside its subfolder.

## Package Names

- Python distribution: `CellGPS`
- Python import package: `cellgps`
- R package/repository: `cellgpsr`
- Windows executable: `cellgps.exe`

The Python package is hosted at <https://github.com/Moldia/CellGPS>. The R package is hosted separately at <https://github.com/Moldia/cellgpsr>. The Windows files are distributed through <https://zenodo.org/records/19482685>.

## What CellGPS does

- Computes searcher-findee distance matrices from spatial coordinates and cell labels.
- Builds cophenetic distance matrices and StructureMap heatmaps from `AnnData` objects or plain `pandas` tables.
- Loads 10x Xenium outputs and prepares them for downstream spatial analysis.
- Provides Xenium loaders backed by `pyXenium.io`, including standard Xenium folders and table bundles with `cells.parquet`, official `*_cell_groups.csv`, and `cell_feature_matrix.h5`.
- Supports transcript-by-cell analysis for locating transcripts relative to cell types.
- Includes memory-optimized workflows for large datasets.
- Provides plotting utilities such as clustered heatmaps, circular dendrograms, and related summary figures.

## Repository layout

- `src/cellgps/`: recommended Python import namespace.
- `src/cellgps/pp`, `src/cellgps/tl`, `src/cellgps/pl`: scverse-style aliases for preprocessing, analysis, and plotting APIs.
- `tests/`: package tests and smoke checks.
- `docs/`: Sphinx documentation.
- `docs/project/`: project notes, changelog, authors, and reviewer guide.
- `CellGPS manuscript/`: curated preprint figure and table notebooks for the main CellGPS/COSTE bioRxiv preprint.
- `manuscripts/`: analysis code for follow-up COSTE extension manuscripts, one self-contained study per subfolder (see each subfolder's README).
- `examples/`: compact usage examples and small example data files.
- `packaging/conda-recipe/`: archived local conda recipe retained for reference.
- `packaging/pyinstaller/`: Windows executable build scripts and PyInstaller assets.

## Installation

Install the current source directly from GitHub:

```bash
python -m pip install git+https://github.com/Moldia/CellGPS.git
```

For local development or reviewer inspection:

```bash
git clone https://github.com/Moldia/CellGPS.git
cd CellGPS
python -m pip install -e .
```

The package requires Python 3.9 or later.

## Quick start from a coordinate table

The minimal input is a table with spatial coordinates and a cell-type column.

```python
import pandas as pd
from cellgps import compute_cophenetic_distances_from_df, plot_cophenetic_heatmap

df = pd.DataFrame(
    {
        "x": [0, 1, 5, 6],
        "y": [0, 1, 5, 6],
        "celltype": ["A", "A", "B", "B"],
    }
)

row_coph, col_coph = compute_cophenetic_distances_from_df(
    df=df,
    x_col="x",
    y_col="y",
    celltype_col="celltype",
)

plot_cophenetic_heatmap(
    row_coph,
    matrix_name="row_coph",
    output_dir="output",
    output_filename="StructureMap_example.pdf",
    sample="Example",
)
```

## Quick start from Xenium output

```python
from cellgps import load_xenium_data, load_xenium_table_bundle, compute_cophenetic_distances_from_adata

# Standard Xenium folder through pyXenium.io
adata = load_xenium_data("/path/to/xenium/run", normalize=False)

# Explicit table-bundle route used for the Atera Xenium benchmark
adata = load_xenium_table_bundle("/path/to/xenium/run", normalize=False)

row_coph, col_coph = compute_cophenetic_distances_from_adata(
    adata,
    cluster_col="Cluster",
    output_dir="output",
)
```

## Useful public entry points

- `load_xenium_data`: load and preprocess Xenium data.
- `load_xenium_table_bundle`: load Xenium data from `cells.parquet` + `*_cell_groups.csv` + `cell_feature_matrix.h5` through `pyXenium.io`.
- `compute_cophenetic_distances_from_df`: compute structure matrices from a coordinate table.
- `compute_weighted_searcher_findee_distance_matrix_from_df`: weighted searcher-findee kernel for entity, pathway, or LR analysis.
- `compute_weighted_cophenetic_distances_from_df`: weighted StructureMap wrapper over the weighted kernel.
- `compute_cophenetic_distances_from_adata`: compute structure matrices from `AnnData`.
- `compute_entity_to_cell_topology`: generalize `t_and_c` from transcripts to arbitrary weighted entities.
- `compute_entity_structuremap`: build StructureMap-style topology among arbitrary weighted entities.
- `plot_cophenetic_heatmap`: generate StructureMap and related clustered heatmaps.
- `transcript_by_cell_analysis`: analyze transcript-to-cell spatial structure at scale.
- `ligand_receptor_topology_analysis`: score sender->receiver ligand-receptor candidates using topology, structure compatibility, and local contact.
- `ligand_receptor_target_consistency`: add a NicheNet-style downstream target-consistency layer.
- `compute_pathway_activity_matrix`: compute rank-based or weighted pathway activities per cell.
- `pathway_topology_analysis`: analyze pathway-to-cell and pathway-to-pathway spatial topology.
- `compute_cophenetic_distances_from_df_memory_opt`: memory-aware alternative for large tables.
- `plot_circular_dendrogram_pycirclize`: circular dendrogram visualization.

## Validation scope

The manuscript-validated scope is the COSTE/SSS workflow and the figure/table analyses mapped in `CellGPS manuscript/` and `docs/cellgps_science_manuscript_code_inventory.md`. Ligand-receptor topology, pathway topology, Visium helpers, GUI entry points and other convenience APIs are included for reuse and development, but should be treated as optional or exploratory unless a manuscript notebook or documentation page explicitly maps them to a reported analysis.

## Notes for reviewers

- The curated figure and table notebooks for the bioRxiv preprint are kept in `CellGPS manuscript/`.
- Raw experimental datasets are not bundled in this repository because of size and distribution constraints. The code expects standard spatial omics outputs such as Xenium folders or tabular coordinate inputs.
- When a `cellgps_tbc_formal_wta/results`-style directory is already available, the LR and pathway topology extensions are designed to reuse its `t_and_c_result_*.csv` and `StructureMap_table_*.csv` outputs as the preferred gene-level topology anchors before falling back to recomputation.
- Xenium loading depends on `pyXenium>=0.4.3`. Visium helpers remain optional through the separate `CellGPS[visium]` extra.
- A short repository walkthrough is available in [docs/project/REVIEWER_GUIDE.md](docs/project/REVIEWER_GUIDE.md).

## Documentation

Sphinx documentation sources are available in [`docs/`](docs/). The manuscript-focused pages introduce the bioRxiv preprint, explain how to use the curated figure/table notebooks, and map each figure and supplementary table to its GitHub code location.

## Citation

If you use CellGPS or reuse the manuscript analysis code, please cite:

> Mengping Long, Taobo Hu, Alexandros Sountoulidis, Christos Samakovlis, and Mats Nilsson (2026). *Cophenetic Spatial Topology Embedding reveals multiscale tissue architecture in spatial omics*. bioRxiv preprint. [doi:10.64898/2026.05.26.727847](https://doi.org/10.64898/2026.05.26.727847). [Version 1 on bioRxiv](https://www.biorxiv.org/content/10.64898/2026.05.26.727847v1).

```bibtex
@article{long2026cellgps,
  title = {Cophenetic Spatial Topology Embedding reveals multiscale tissue architecture in spatial omics},
  author = {Long, Mengping and Hu, Taobo and Sountoulidis, Alexandros and Samakovlis, Christos and Nilsson, Mats},
  journal = {bioRxiv},
  year = {2026},
  doi = {10.64898/2026.05.26.727847},
  url = {https://www.biorxiv.org/content/10.64898/2026.05.26.727847v1}
}
```

## License

This project is released under the MIT License. See [LICENSE](LICENSE).
