pylibmc-manylinux
=====================
Example for building pylibmc wheels for Linux with Travis-CI

[![Build Status](https://travis-ci.org/bxm156/pylibmc-manylinux.svg?branch=master)](https://travis-ci.org/bxm156/pylibmc-manylinux)


This is an example of how to use Travis-CI to build
[PEP 513](https://www.python.org/dev/peps/pep-0513/)-compatible manylinux1
wheels for pylibmc. It supports both Python 2 and 3 on 32 and 64 bit linux
architectures.

Because these wheels need to be compiled on CentOS 5, this example uses Docker
running on Travis-CI to compile (you don't need to use docker at all to _use_
these wheels, it's just to compile them). The docker-based build environment
images are:

- 64-bit image (x86-64): ``quay.io/pypa/manylinux1_x86_64`` [![Docker Repository on Quay](https://quay.io/repository/pypa/manylinux1_x86_64/status "Docker Repository on Quay")](https://quay.io/repository/pypa/manylinux1_x86_64)
- 32-bit image (i686): ``quay.io/pypa/manylinux1_i686`` [![Docker Repository on Quay](https://quay.io/repository/pypa/manylinux1_i686/status "Docker Repository on Quay")](https://quay.io/repository/pypa/manylinux1_i686)


Continuous integration setup with Travis + Docker
-------------------------------------------------

The `.travis.yml` file in this repository sets up the build environment. The
resulting build logs can be found at

  https://travis-ci.org/bxm156/pylibmc-manylinux

The `.travis.yml` file instructs Travis to pull the PyPA docker images, 
install package dependencies, libmemcached 1.0.18, and then build a pylibmc wheel. These
wheels link against an external library. So to create self-contained wheels,
the build script runs the wheels through
[`auditwheel`](https://pypi.python.org/pypi/auditwheel), which copies the external
library into the wheel itself, so that users won't need to install any extra non-PyPI
dependencies, such as:
* libsasl2.so
* libz.so
* libresolv.so
* libmemcached.so
