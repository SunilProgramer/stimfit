# We moved to GitHub #
New [releases](https://github.com/neurodroid/stimfit/releases) and [code](https://github.com/neurodroid/stimfit) are now available from [GitHub](https://github.com).

This is because Google Code have [deprecated their Download service](http://google-opensource.blogspot.co.uk/2013/05/a-change-to-google-code-download-service.html). Not only was the download service very important to us, but it's also an indication that Google may be retiring Google Code in the future.

# Stimfit #

## Introduction ##
Stimfit is a free, fast and simple program for viewing and analyzing electrophysiological data. It's currently available for GNU/Linux, Mac OS X and Windows. It features an embedded Python shell that allows you to extend the program functionality by using numerical libraries such as [NumPy](http://numpy.scipy.org) and [SciPy](http://www.scipy.org). A standalone Python module for file i/o that doesn't depend on the graphical user interface is also available.

We'd appreciate if you could cite the following publication when you use Stimfit for your research:

Guzman SJ, Schlögl A, Schmidt-Hieber C (2014) Stimfit: quantifying electrophysiological data with Python. _Front Neuroinform_ [doi: 10.3389/fninf.2014.00016](http://www.frontiersin.org/Journal/10.3389/fninf.2014.00016/abstract)

## Supported file types ##
  * Read/write:  CFS binary data, HDF5 files, Axon text files,  ASCII files
  * Read-only: Axon binary files versions 1 and 2 (pClamp 9 and 10, `*`.abf), Axograph files (`*`.axgd, `*`.axgx), HEKA files (`*`.dat, from version 0.10)
  * Write-only: Igor binary waves (`*`.ibw)
Support for other file types may be implemented upon request.
## Building and installing ##
### Windows ###
The Windows version, including the python-stfio module, is available [here](https://github.com/neurodroid/stimfit/releases).
### OS X ###
Stimfit for OS X is available through [MacPorts](http://www.macports.org). After [installation](https://www.macports.org/install.php), run
```
$ sudo port install stimfit py27-stfio
```
We don't supply installation packages for OS X any longer because of fragmentation issues with OS X versions (10.5--10.9), Python versions (2.x, 3.x) and architectures (i386, x86\_64, ppc).
### GNU/Linux ###
On Debian and Ubuntu systems, you can get Stimfit and the stfio module from the standard repositories:
```
$ sudo apt-get install stimfit python-stfio
```
More recent versions can be found on [the Debian Neuroscience Repository](http://neuro.debian.net/index.html), and there's [a bleeding-edge ppa for Ubuntu](https://launchpad.net/~christsc-gmx/+archive/neuropy) on launchpad.
### Build from source ###
The source code for the latest release can be obtained from [this ppa](https://launchpad.net/~christsc-gmx/+archive/neuropy/+packages). Or you can get the latest source from our [git repository](https://code.google.com/p/stimfit/source/checkout).
José Guzman has contributed [building instructions](http://www.stimfit.org/doc/sphinx/linux_install_guide/index.html) for GNU/Linux, which are part of the [documentation](http://www.stimfit.org/doc/sphinx/index.html). Build instructions for OS X can be found [here](http://www.stimfit.org/doc/sphinx/osx_install_guide/index.html).

Questions and comments can be posted to the [Google group](http://groups.google.com/group/stimfit).

![http://www.stimfit.org/Home_files/shapeimage_2.jpg](http://www.stimfit.org/Home_files/shapeimage_2.jpg)