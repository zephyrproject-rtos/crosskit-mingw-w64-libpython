# mingw-w64-libpython

This repository contains the pre-generated `libpython*.dll` import libraries
and headers for the MinGW-w64, in order to facilitate Linux-to-Windows cross
compilation of the programs that depend on the `libpython*.dll` (e.g. GDB).

Note that the import libraries in this repository do not target the MSYS2
Python DLLs; instead, they target the [native Windows Python distribution from
the official Python website](https://www.python.org/downloads/windows/).

__SUPERHACK WARNING!__ This repository also contains a special shell script
(`${LIBPYTHON_KIT_ROOT}/bin/python`) that emulates the GDB build system
`python-config.py` to allow the Linux GDB build system to resolve the
`include` and `lib` paths for the Windows Python.

## Generate a new kit

Follow the steps below to generate a new cross `libpython` kit:

1. Download and install x86-64 Python distribution from the
   [Python website](https://www.python.org/downloads/windows/).

2. Create a kit root directory (referred to as `${LIBPYTHON_KIT_ROOT}` in this
   document).

3. Copy Python `include` directory to the kit root directory, such that the
   contents of the `include` directory resides in
   `${LIBPYTHON_KIT_ROOT}/include`.

4. Copy Python `python[maj][min].dll` (e.g. `python38.dll` for Python 3.8) to
   `${LIBPYTHON_KIT_ROOT}/lib`.

5. Run the following commands in `${LIBPYTHON_KIT_ROOT}/lib` directory to
   generate an import library (using Python 3.8 as example):

    ```
    gendef python38.dll
    x86_64-w64-mingw32-dlltool -D python38.dll -d python38.def -l libpython38.a
    ```
