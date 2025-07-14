Clock Source Configuration
==========================

This section describes how to configure the clock source and system frequency
for WCH microcontrollers in your PlatformIO project.

Overview
--------

The clock source determines the oscillator and clock path used to drive the system. Valid clock sources include:

- ``hsi``       : Internal High-Speed Internal oscillator
- ``hse``       : External High-Speed External oscillator
- ``hsilp``     : Internal High-Speed Internal Low-Power oscillator (only CH32L10x series)
- ``hsi+pll``   : HSI oscillator with PLL multiplier
- ``hse+pll``   : HSE oscillator with PLL multiplier

For CH32 series, PLL frequencies must match a set of valid values depending
on the MCU series, since the ``system_chxxxx.c`` files from the WCH SDK can only handle discrete choices.

Configuration
-------------

To change the clock source or system frequency, set ``board_build.clock_source`` and ``board_build.f_cpu`` in your ``platformio.ini``.

Examples
--------

Use HSI oscillator at default internal frequency (usually 8 MHz):

.. code:: ini

    [env:genericCH32V103C8T6]
    platform = ch32v
    board = genericCH32V103C8T6
    board_build.clock_source = hsi

Use HSE oscillator plus PLL at 144 MHz:

.. code:: ini

    [env:genericCH32V307RCT6]
    platform = ch32v
    board = genericCH32V307RCT6
    board_build.clock_source = hse+pll
    board_build.f_cpu = 144000000L

Validation
----------

The build system validates your configuration against a list of allowed
clock frequencies specific to each chip series. Invalid values will result
in a clear and descriptive build error.

Defined Macros
--------------

Depending on your configuration, the following macros may be defined during build:

- ``SYSCLK_FREQ_HSI``, ``SYSCLK_FREQ_HSE``, or ``SYSCLK_FREQ_HSI_LP`` for direct oscillator sources
- ``SYSCLK_FREQ_<XX>MHz_HSI`` or ``SYSCLK_FREQ_<XX>MHz_HSE`` for PLL-based configurations on CH32 series
- ``FREQ_SYS`` for CH56x and similar chips, set directly to the system frequency in Hz
