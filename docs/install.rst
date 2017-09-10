INSTALLATION
============

OSX
---

*  Scripts provided are based off of homebrew (copy and paste into terminal).  Should not install if you already have MacPorts.
*  No support for MacPorts is provided.

.. code-block:: bash

   # Install Homebrew
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

   # From:  https://www.karambelkar.info/2016/10/gdal-2-on-mac-with-homebrew/
   brew unlink gdal
   brew tap osgeo/osgeo4mac && brew tap --repair
   brew install jasper netcdf # gdal dependencies
   brew install gdal2 --with-armadillo \
   --with-complete --with-libkml --with-unsupported
   brew link --force gdal2
   
   brew install spatialindex


Debian/Ubuntu
-------------

.. code-block:: bash

   # install the system dependencies
   sudo add-apt-repository -y ppa:ubuntugis/ppa
   sudo apt-get update
   sudo apt-get install -y libgdal-dev
   sudo apt-get install -y libspatialindex-dev
   sudo apt-get install -y libblas-dev \
   liblapack-dev libatlas-base-dev gfortran libfreetype6-dev

   # Optional: create python virtual environment
   virtualenv venv
   source venv/bin/activate

   # update GDAL version in requirements.txt to match system version
   # (GDAL 2.0.2 doesn’t seem to be available in any PPA)
   # GDAL==1.11.2

   # Configure GDAL before installing
   export CPLUS_INCLUDE_PATH=/usr/include/gdal
   export C_INCLUDE_PATH=/usr/include/gdal

   # install python dependencies
   pip install numpy scipy
   pip install -r requirements-dev.txt
   pip install -r requirements.txt
   pip install -e /path/to/pyspatial

   # run the tests
   nosetests -v


Python
------

PyPI
~~~~


.. code-block:: bash

	pip install pyspatial


Latest
~~~~~~
.. code-block:: bash

   git clone https://github.com/granularag/pyspatial.git
   cd pyspatial
   pip install -r requirements.txt
   pip install .


Appendix
----------

Compiling GDAL from source
~~~~~~~~~~~~~~~~~~~~~~~~~~

* A good overview is provided here: https://docs.djangoproject.com/en/1.9/ref/contrib/gis/install/geolibs/
* More detailed information can be found here: https://trac.osgeo.org/gdal/wiki/BuildHints


1. If you don't have root access, you should download the source and build packages like
  * $ ./configure --prefix ~/local
* To make the binaries available add the following to your bashrc

.. code-block:: bash

   export HOME_DIR=/my/home/dir
   export PATH=$PATH:$HOME_DIR/local/bin

* To build gdal (assume geos installed in /usr/local), in non-standard localtion:
  * $ export HOME_DIR=/my/home/dir
  * $ ./configure --enable-64bit --prefix $HOME_DIR/local --with-includes=$HOME_DIR/local/include/ --with-libs=$HOME_DIR/local/lib/ --with-sqlite=no --with-geos=/usr/local/bin/geos-config --with-opengl=no --with-cairo=no --with-freetype=no --with-lapack --with-blas --with-readline
* In your scripts/bashrc:


.. code-block:: bash

   export HOME_DIR=/my/home/dir
   export GDALHOME=$HOME_DIR/local/
   export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$USER_HOME/local/lib/
