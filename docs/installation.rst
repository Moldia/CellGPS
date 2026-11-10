.. highlight:: shell

============
Installation
============

From GitHub
-----------

Install directly from the public repository:

.. code-block:: console

   $ python -m pip install git+https://github.com/Moldia/CellGPS.git

For local inspection
--------------------

Clone the repository and install it in editable mode:

.. code-block:: console

   $ git clone https://github.com/Moldia/CellGPS.git
   $ cd CellGPS
   $ python -m pip install -e .

Requirements
------------

* Python 3.9 or newer
* AnnData and the scientific Python dependencies declared in ``pyproject.toml``
* ``pyXenium`` for optional Xenium I/O; normalization additionally uses Scanpy

For migration from historical distributions and imports, see
``docs/project/MIGRATION.md`` in the public repository. The current installation
route uses only the company source; package-registry publication is separate.

Optional extras
---------------

Install Xenium support:

.. code-block:: console

   $ pip install -e .[xenium]

Install Visium support:

.. code-block:: console

   $ pip install -e .[visium]

Related Packages
----------------

* Python repository: https://github.com/Moldia/CellGPS
* R package repository: https://github.com/Moldia/cellgpsr
* Windows files: https://zenodo.org/records/19482685

The Python repository is the primary, full implementation and includes all
CellGPS functions plus the complete bioRxiv preprint reproduction code. The R
package and Windows files provide only the core functionality.
