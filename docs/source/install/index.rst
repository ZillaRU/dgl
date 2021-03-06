Install DGL
===========

This topic explains how to install DGL. We recommend installing DGL by using ``conda`` or ``pip``.

System requirements
-------------------
DGL works with the following operating systems:

* Ubuntu 16.04
* macOS X
* Windows 10

DGL requires Python version 3.6, 3.7, 3.8 or 3.9.

DGL supports multiple tensor libraries as backends, e.g., PyTorch, MXNet. For requirements on backends and how to select one, see :ref:`backends`.

Starting at version 0.3, DGL is separated into CPU and CUDA builds.  The builds share the
same Python package name. If you install DGL with a CUDA 9 build after you install the
CPU build, then the CPU build is overwritten.

Install from Conda or Pip
-------------------------

Check out the `Get Started page <https://www.dgl.ai/pages/start.html>`_.

.. _install-from-source:

Install from source
-------------------
Download the source files from GitHub.

.. code:: bash

   git clone --recurse-submodules https://github.com/dmlc/dgl.git

(Optional) Clone the repository first, and then run the following:

.. code:: bash

   git submodule update --init --recursive

Linux
`````

Install the system packages for building the shared library. For Debian and Ubuntu
users, run:

.. code:: bash

   sudo apt-get update
   sudo apt-get install -y build-essential python3-dev make cmake

For Fedora/RHEL/CentOS users, run:

.. code:: bash

   sudo yum install -y gcc-c++ python3-devel make cmake

Build the shared library. Use the configuration template ``cmake/config.cmake``.
Copy it to either the project directory or the build directory and change the
configuration as you wish. For example, change ``USE_CUDA`` to ``ON`` will
enable a CUDA build. You could also pass ``-DKEY=VALUE`` to the cmake command
for the same purpose.

- CPU-only build
   .. code:: bash

      mkdir build
      cd build
      cmake ..
      make -j4
- CUDA build
   .. code:: bash

      mkdir build
      cd build
      cmake -DUSE_CUDA=ON ..
      make -j4

Finally, install the Python binding.

.. code:: bash

   cd ../python
   python setup.py install

macOS
`````

Installation on macOS is similar to Linux. But macOS users need to install build tools like clang, GNU Make, and cmake first. These installation steps were tested on macOS X with clang 10.0.0, GNU Make 3.81, and cmake 3.13.1.

Tools like clang and GNU Make are packaged in **Command Line Tools** for macOS. To
install, run the following:

.. code:: bash

   xcode-select --install

To install other needed packages like cmake, we recommend first installing
**Homebrew**, which is a popular package manager for macOS. To learn more, see the `Homebrew website <https://brew.sh/>`_.

After you install Homebrew, install cmake.

.. code:: bash

   brew install cmake

Go to root directory of the DGL repository, build a shared library, and
install the Python binding for DGL.

.. code:: bash

   mkdir build
   cd build
   cmake -DUSE_OPENMP=off -DCMAKE_C_FLAGS='-DXBYAK_DONT_USE_MAP_JIT' -DCMAKE_CXX_FLAGS='-DXBYAK_DONT_USE_MAP_JIT' ..
   make -j4
   cd ../python
   python setup.py install

Windows
```````

You can build DGL with MSBuild.  With `MS Build Tools <https://go.microsoft.com/fwlink/?linkid=840931>`_
and `CMake on Windows <https://cmake.org/download/>`_ installed, run the following
in VS2019 x64 Native tools command prompt.

- CPU only build
  .. code::

     MD build
     CD build
     cmake -DCMAKE_CXX_FLAGS="/DDGL_EXPORTS" -DCMAKE_CONFIGURATION_TYPES="Release" -DDMLC_FORCE_SHARED_CRT=ON .. -G "Visual Studio 16 2019"
     msbuild dgl.sln /m
     CD ..\python
     python setup.py install
- CUDA build
  .. code::

     MD build
     CD build
     cmake -DCMAKE_CXX_FLAGS="/DDGL_EXPORTS" -DCMAKE_CONFIGURATION_TYPES="Release" -DDMLC_FORCE_SHARED_CRT=ON -DUSE_CUDA=ON .. -G "Visual Studio 16 2019"
     msbuild dgl.sln /m
     CD ..\python
     python setup.py install

Optional Flags
``````````````

- If you are using PyTorch, you can add ``-DBUILD_TORCH=ON`` flag in CMake
  to build PyTorch plugins for further performance optimization.  This applies for Linux,
  Windows, and Mac.
