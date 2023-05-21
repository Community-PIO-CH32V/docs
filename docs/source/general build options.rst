General Build Options
=====================

FPU / Hard Float
----------------

The CH32V30x series of chips has a QingKe V4F core that's capable of native floating point instructions. This is by-default not used.

To activate it, please change the `march` (default: `rv32imacxw`, affects generating FPU instructions) and `mabi` (default: `ilp32`, affects the calling convention and passing of arguments via registers) as follows:

.. code:: ini

    board_build.march = rv32imafcxw
    board_build.mabi = ilp32f

Please refer to https://www.sifive.com/blog/all-aboard-part-1-compiler-args for detail.

Linker Script
-------------

The linker script can be manually set by using

.. code:: ini

    board_build.ldscript = path/to/linkerscript.ld
