# Moving to CellGPS

The formal project name is **CellGPS**. The canonical public source is
[Moldia/CellGPS](https://github.com/Moldia/CellGPS), maintained in the Moldia
organization. Use this repository for current development, documentation and
the scverse ecosystem application.

## Install the company source

Use a fresh Python environment to avoid overlapping files from historical
distributions, then install the public source:

```console
python -m pip install git+https://github.com/Moldia/CellGPS.git
```

For reproducible analyses, replace the repository URL above with
`git+https://github.com/Moldia/CellGPS.git@<commit-sha>` and record that commit.
Optional support is installed from the same source, for example:

```console
python -m pip install "CellGPS[xenium] @ git+https://github.com/Moldia/CellGPS.git"
```

The supported import remains:

```python
import cellgps
```

The current company source does not provide the historical `sfplot` import
namespace. Update imports in existing scripts to `cellgps` and verify those
scripts before using them for new analyses. Private compatibility helper names
from the old implementation are not part of the public API.

## Existing analyses and publications

Keep the original environments, versions and paths needed to reproduce older
analyses. Historical releases and manuscript paths remain historical records;
they are not the source of the current company implementation.

The company baseline retains the nearest-neighbor and cophenetic computation
used by the previous implementation. Some default figure titles and output
filenames use CellGPS instead of Cell-GPS, so scripts that collect output files
by exact name may need adjustment. Scientific results still require validation
in the analysis environment.

The formal project name and a package-registry distribution identifier are
separate. A new `CellGPS` PyPI publication is outside this source migration;
the install command above deliberately uses only the public company repository.

Retain the existing author credits, MIT copyright notice and COSTE preprint
citation. Moldia maintenance does not replace those credits.
