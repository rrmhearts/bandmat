bandmat
=======

|Linux build status| |Windows build status|

.. |Linux build status| image:: https://github.com/rrmhearts/bandmat/actions/workflows/python-package.yml/badge.svg
   :alt: Linux build status
   :target: https://github.com/rrmhearts/bandmat/actions/workflows/python-package.yml
.. |Windows build status| image:: https://ci.appveyor.com/api/projects/status/cy8ouawd9bnus0ai/branch/master?svg=true
   :alt: Windows build status
   :target: https://ci.appveyor.com/project/MattShannon/bandmat/branch/master

This package provides a simple banded matrix library for python.
It supports banded matrix-vector and matrix-matrix multiplication, converting
between full and banded matrix representations, and certain linear algebra
operations on banded matrices.
It builds on the excellent numpy and scipy packages, which have limited support
for banded matrix operations.

Overview
--------

A banded matrix is a matrix where only the diagonal, a number of superdiagonals
and a number of subdiagonals are non-zero.
The well-known BLAS interface and LAPACK library for linear algebra define
several banded matrix operations, and some of these, such as banded Cholesky
decomposition, are wrapped in the excellent python package scipy, specifically
in scipy.linalg.
The bandmat package re-uses the banded matrix representation used by BLAS,
LAPACK and scipy.linalg, wrapping it in a lightweight class for ease of use.
See the docstring for the BandMat class for full details of the representation
used.

The bandmat package provides:

- a lightweight class wrapping the LAPACK-style banded matrix representation.
  This class keeps track of things like bandwidths to allow a more direct
  coding style when working with banded matrices.
- some basic banded matrix operations not present in scipy.
  For example, banded matrix-vector multiplication is defined by BLAS but not
  wrapped by scipy, and banded matrix-matrix multiplication is not defined in
  BLAS or in scipy.
  The bandmat package contains C implementations of these operations written in
  cython.
- helper functions for converting between full and banded matrix
  representations.
- certain linear algebra operations on banded matrices, including computing the
  band of the inverse of a banded matrix.

Only square banded matrices are supported by this package.

License
-------

Please see the file ``License`` for details of the license and warranty for
bandmat.

Installation
------------

For most purposes the simplest way to install bandmat is to use pip.
For example in Debian and Ubuntu::

    # (for python3, use "python3-numpy python3-scipy" instead)
    sudo apt-get install python-numpy python-scipy
    sudo pip install bandmat

The first command installs numpy and scipy from the system repository, since
installing numpy and scipy using pip is generally not recommended.
The second command installs the latest released version of
`bandmat on PyPI <https://pypi.python.org/pypi/bandmat>`_, together with any
currently uninstalled python packages required by bandmat.

bandmat can also be installed in a virtualenv::

    # (for python3, use "python3-numpy python3-scipy" instead)
    sudo apt-get install python-numpy python-scipy
    virtualenv --system-site-packages env
    env/bin/pip install bandmat

The latest development version of bandmat is available from a github repository
(see below).

To check that bandmat is installed correctly you can run the test suite::

    python -m unittest discover bandmat

Examples
--------

See the package docstring (run ``import bandmat as bm; help(bm)`` in the python
interpreter) for some examples of usage.
The python script ``example.py`` also contains some simple examples of usage.
To run it::

    python example.py

Development
-----------

The source code is hosted in the
`bandmat github repository <https://github.com/MattShannon/bandmat>`_.
To obtain the latest source code using git::

    git clone git://github.com/MattShannon/bandmat.git

Development is in fact done using `darcs <http://darcs.net/>`_, with the darcs
repository converted to a git repository using
`darcs-to-git <https://github.com/purcell/darcs-to-git>`_.

To install any currently uninstalled python packages required by bandmat::

    # (for python3, use "cython3 python3-numpy python3-scipy" instead)
    sudo apt-get install cython python-numpy python-scipy
    sudo pip install -r requirements.txt

To compile the cython part of bandmat in the current directory::

    python setup.py build_ext --inplace

Local install of built packages::
    python setup.py install

This command must be run after every modification to the source ``.pyx`` files.

A note on ``setup.py``
----------------------

The included ``setup.py`` file operates in one of two modes depending on
whether or not the file ``dev`` is present in the project root directory.
In development mode (``dev`` present, as for the github repository), the
``build_ext`` command uses cython to compile cython modules from their ``.pyx``
source, and the ``sdist`` command is modified to first use cython to compile
cython modules from their ``.pyx`` source to ``.c`` files.
In distribution mode (``dev`` absent, as for source distributions such as the
code on PyPI), the ``build_ext`` command uses a C compiler to directly compile
cython modules from the corresponding ``.c`` files.
This approach ensures that source distributions can be installed on systems
without cython or with an incompatible version of cython, while ensuring that
distributed ``.c`` files are always up-to-date and that the source ``.pyx``
files are used instead of ``.c`` files during development.

The author would welcome any suggestions for more elegant ways to achieve a
similar effect to the approach described above!

Bugs
----

Please use the
`issue tracker <https://github.com/MattShannon/bandmat/issues>`_ to submit bug
reports.

Contact
-------

The author of bandmat is `Matt Shannon <mailto:matt.shannon@cantab.net>`_.

