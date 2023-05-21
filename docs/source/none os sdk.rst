None OS SDK
===========

When building with ``framework = noneos-sdk``, several build options are available for the ``platformio.ini``.

Link Time Optimization (LTO)
----------------------------

To activate LTO in the compiling and linking stage, use

.. code:: ini

    board_build.use_lto = yes

Startup File
------------

By default, a slightly modified startup file (e.g., ``startup_ch32v30x_D8.S``) will be used. 
If you want to use a startup file that you explicitly placed in the project, turn off the compilation of the built-in one using

.. code:: ini

    board_build.use_builtin_startup_file = no

System / Startup Code
---------------------

By default, the built-in ``system_ch32v<series>.c`` will be used (`example <https://github.com/Community-PIO-CH32V/framework-wch-noneos-sdk/blob/main/System/ch32v30x/system_ch32v30x.c>`__). This file contains the clock setup code and by-default uses the HSE as clock source,
thus needing an external crystal oscillator connected to the chip to function correctly.

If you want to use a system file that you explicitly placed in the project that has the correct settings, turn off the compilation of the built-in one using

.. code:: ini

    board_build.use_builtin_system_code = no

Debug Code
----------

By default, the built-in ``debug.c`` will be compiled in the project (`example <https://github.com/Community-PIO-CH32V/framework-wch-noneos-sdk/blob/main/Debug/ch32v30x/debug.c>`__). 
The code in this file contains

1. delay functions
2. UART initialization functions 
3. ``_write`` implementation (for making e.g. ``printf`` work, writes data for ``stdout`` to actual UART registers) 
4. ``_sbrk`` implementation (needed for ``malloc`` and ``free`` functions to allocate new memory)

While this is convenient as default, in your project you might want to have full control over these implementations.

To turn off the compilation of the built-in debug implementation, use

.. code:: ini

    board_build.use_builtin_debug_code = no

C++ Support
-----------

By default, C++ support is disabled.

To be able to use C++ properly, meaning that constructors of global objects are called etc., 
one would usually need to do adaptions to the startup file (calling ``__libc_init_array``) and add a ``_fini`` and ``_init`` implementation.

To make things easier, we have already done these modifications in an activatable way. To turn on C++ support, simple use

.. code:: ini

    board_build.cpp_support = yes

This technically defines ``__PIO_CPP_SUPPORT__`` which the built-in startup file will react to
and also compiles our ``cpp_support.c`` file with ``_weak`` overridable ``_fini`` and ``_init`` functions.