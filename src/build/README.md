# Building from Source

## Cloning the source tree

Ouinet uses Git submodules, thus to properly clone it, use:

    $ git clone --recursive https://github.com/equalitie/ouinet.git

You can also clone and update the modules separately:

    $ git clone https://github.com/equalitie/ouinet.git
    $ cd ouinet
    $ git submodule update --init --recursive

## Build requirements (desktop)

To build Ouinet natively on your system, you will need the following software
to be already available:

* CMake 3.5+
* `g++` capable of C++14
* The [Boost library](http://www.boost.org/) 1.67+

Assuming that `<SOURCE DIR>` points to the directory where the
`CMakeLists.txt` file is, and `<BUILD DIR>` is a directory of your choice
where all (even temporary) build files will go, you can build Ouinet with:

    $ mkdir -p <BUILD DIR>
    $ cd <BUILD DIR>
    $ cmake <SOURCE DIR>
    $ make

However, we encourage you to use a Vagrant environment for development, or
Docker containers for deploying a Ouinet client or an injector.  These have a
different set of requirements.  See the corresponding sections below for
further instructions on Vagrant and Docker.

## Running integration tests

The Ouinet source comes with a set of integration tests.  To run them you will
need the [Twisted](https://twistedmatrix.com/) Python framework.

If you already built Ouinet from `<SOURCE DIR>` into `<BUILD DIR>` (see
above), you can run the tests as follows:

    $ export OUINET_REPO_DIR=<SOURCE DIR>
    $ export OUINET_BUILD_DIR=<BUILD DIR>
    $ ./scripts/run_integration_tests.sh
