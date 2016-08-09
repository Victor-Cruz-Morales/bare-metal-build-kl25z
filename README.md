# Achieve build freedom

The embedded software development world is still very strongly tied to
Integrated Development Environments (IDEs).

While very convenient for beginners, they can be a burden for:

* **Versioning** (Which files to commit among the ones generated by the IDE)
* **Multiple builds** (IDE usually don't support more than one build. It is then
impossible to define a build for the main program and a build for unit tests)
* **Reproducible builds** (related to versioning, i.e. which files are required to
  have a consisten build accross different machines)
* **Continuous testing and deployment** (cannot run IDE in a shell environment)

They also usually suffer from:

* Poor UI and ergonomy
* Poor text editing interface
* Bugs (preventing builds)
* Not portable

The main interest in using an IDE is to use the debugging interface, which is
a key tool for software development in general. But other than that, it is
interesting to look for alternatives.

# The workflow

This repository provides a different workflow that does not rely on
an IDE for embedded-software (also called *bare-metal*) development, and solves
all issues described previously.

Ultimately, an IDE is 3 things:

* A text editor : You know what that is.
* A build system : A tool that manages the toolchain (a collection of programs
  used for building the code).
* A debugging interface: A graphical interface to facilitate application
debugging during development.

These components can be easily replaced. In this example, the following tools
were chosen:

* **Text-editor**: Your choice. My favorite: [`Atom`](https://atom.io/).
Also, check out [`Sublime`](https://www.sublimetext.com/).
* **Build-system**: [Ninja](https://ninja-build.org/).
Ninja let us write very readable build scripts, is extremely fast and still
lets us have low-level control on the build.
* **Debugging interface**: I won't search for an alternative to this
component yet, because the debugger can be used in a shell with enough ease.

# Preparation

First install [Ninja](https://ninja-build.org/) and
[GNU GCC ARM Embedded toolchain](https://launchpad.net/gcc-arm-embedded).

Make sure both of them are well installed (= available in the PATH) by running:

```
ninja -v
arm-none-eabi-gcc -v
```

For this example, we will use an [NXP freedom KL25Z board](http://www.nxp.com/products/software-and-tools/hardware-development-tools/freedom-development-boards/freedom-development-platform-for-kinetis-kl14-kl15-kl24-kl25-mcus:FRDM-KL25Z).

In doubt, change the bootloader of the KL25Z with the latest available from
[pemicro](http://www.pemicro.com/opensda/) under *OpenSDA Firmware (MSD & Debug)*.

*(If you find a compatible and more open bootloader, please feel free to fork
this README and make a PR, it will be very appreciated)*.

To update the bootloader, disconnect the board, hold the press-button and connect
it (on the OpenSDA USB port). The board should be visible now as a drive called
`BOOTLOADER`. Drag and drop the dowloaded file on this drive, disconnect then
reconnect the board (on the same USB port) and it should now have the title
`FRDM-KL25Z`.

## Project structure
To update
```
\mylib
  \headers
    mylib.hpp
  \cpp
    mylib.cpp
\test
  \headers
    test.hpp
  \cpp
    test_A.cpp
    test_B.cpp
\platform
   memory.hpp
   linker files ?
\main
  main.cpp
premake5.lua
```
Folders `mylib` and `test` contain sources of the developed library
and the tests that go with it.

In the `main` folder, `main.cpp` implements the final program,
that relies on `mylib`.

The `platform` folder is specific to embedded software.
It contains information provided by the microcontroller vendor such as
memory size and location, ... ?

See Additional resources to find out where to get these files.

The `build.ninja` is our build script.
A build script may seem rudimentary for people accostumed to IDEs.
However, it is simple to read, edit and helps guarantee reproducible builds,
since it is the only source of truth for the build configuration.

*To be continued...*
