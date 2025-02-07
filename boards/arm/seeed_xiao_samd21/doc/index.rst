.. _seeed_xiao_samd21:

Seeed Studio XIAO SAMD21 (Seeeduino XIAO)
#########################################

.. admonition:: Downstream Copy!
   :class: note

   This board description is a copy from Zephyr with a slightly changed name
   and will be used for further development, improvement and preparation of
   changes for Zephyr within Bridle. However, the original board description
   still lives within the Zephyr namespace under the original board name:
   :ref:`zephyr:seeeduino_xiao`.

Overview
********

The Seeed Studio XIAO SAMD21 (Seeeduino XIAO) is a tiny (20 mm x 17.5 mm)
ARM development board with onboard LEDs, USB port, and range of digital
or analog I/O broken out onto 14 pins.

.. image:: img/seeed_xiao_samd21.jpg
     :align: center
     :alt: Seeed Studio XIAO SAMD21 (Seeeduino XIAO)

Hardware
********

- `ATSAMD21G18A`_ ARM Cortex-M0+ processor at 48 MHz
- 32.768 kHz crystal oscillator
- 256 KiB flash memory and 32 KiB of RAM
- 3 user LEDs (L/Rx/Tx)
- One reset pad (solderable), beside an free GND pad
- |Seeed XIAO| header
- Native USB port

Supported Features
==================

The :code:`seeed_xiao_samd21` board configuration supports the following
hardware features:

+-----------+------------+------------------------------------------+
| Interface | Controller | Driver/Component                         |
+===========+============+==========================================+
| ADC       | on-chip    | Analogue to digital converter            |
+-----------+------------+------------------------------------------+
| DAC       | on-chip    | Digital to analogue converter            |
+-----------+------------+------------------------------------------+
| DMA       | on-chip    | Direct memory access                     |
+-----------+------------+------------------------------------------+
| Flash     | on-chip    | Can be used with LittleFS to store files |
+-----------+------------+------------------------------------------+
| GPIO      | on-chip    | I/O ports                                |
+-----------+------------+------------------------------------------+
| HWINFO    | on-chip    | Hardware info                            |
+-----------+------------+------------------------------------------+
| I2C       | on-chip    | Inter-Integrated Circuit                 |
+-----------+------------+------------------------------------------+
| NVIC      | on-chip    | nested vector interrupt controller       |
+-----------+------------+------------------------------------------+
| PWM       | on-chip    | Pulse Width Modulation                   |
+-----------+------------+------------------------------------------+
| SPI       | on-chip    | Serial Peripheral Interface ports        |
+-----------+------------+------------------------------------------+
| SYSTICK   | on-chip    | systick                                  |
+-----------+------------+------------------------------------------+
| USART     | on-chip    | Serial ports                             |
+-----------+------------+------------------------------------------+
| USB       | on-chip    | USB device                               |
+-----------+------------+------------------------------------------+
| WDT       | on-chip    | Watchdog                                 |
+-----------+------------+------------------------------------------+

Other hardware features are not currently supported by Zephyr.

The default configuration can be found in the Kconfig file
:bridle_file:`boards/arm/seeed_xiao_samd21/seeed_xiao_samd21_defconfig`.

Board Configurations
====================

The :code:`seeed_xiao_samd21` board can be configured for the following
different use cases.

.. rubric:: :command:`west build -b seeed_xiao_samd21`

Use the serial port SERCOM5 over |Seeed XIAO| header as Zephyr console
and for the shell.

.. rubric:: :command:`west build -b seeed_xiao_samd21 -S usb-console`

Use the native USB device port with CDC-ACM as Zephyr console
and for the shell.

Connections and IOs
===================

The `Seeed Studio XIAO SAMD21 wiki`_ has detailed information about the board
including `pinouts <Seeed Studio XIAO SAMD21 Pinouts_>`_ and the
`schematic <Seeed Studio XIAO SAMD21 Schematic_>`_. There are also design data
for `Eagle <Seeed Studio XIAO SAMD21 Design Data for Eagle_>`_ and
`KiCAD <Seeed Studio XIAO SAMD21 Design Data for KiCAD_>`_.

System Clock
============

The SAMD21 MCU is configured to use the 32.768 kHz external crystal with the
on-chip PLL generating the 48 MHz system clock. The internal APB and GCLK unit
are set up in the same way as the upstream Arduino libraries.

GPIO (PWM) Ports
================

The SAMD21 MCU has 2 GPIO ports, 3 PWM able Timer/Capture-Counter (TCC) and
2 simple Timer/Counter (TC). On the Seeed Studio XIAO SAMD21, TCC2 channel 1
is available on first user LED (L), all other user LEDs can be controlled
as GPIO. Only if :kconfig:option:`CONFIG_PWM_SAM0_TCC` is enabled then the
first user LED (L) is driven by TCC2 instead of by GPIO. All channels of
TCC0 and TCC1 are available on the |Seeed XIAO| header.

ADC/DAC Ports
=============

The SAMD21 MCU has 1 DAC and 1 ADC. On the Seeed Studio XIAO SAMD21, the
DAC voltage output (VOUT) is available on A0 of the |Seeed XIAO| header. The
ADC channels 4 and 18 are available on A1 and A2 of the |Seeed XIAO| header.
Whenever other GPIO (PWM) or serial ports are not needed and are disabled by
DT overlays, up to 11 ADC channels can be configured according to the next
table (default function in bold).

+------------------+--------+-----------+----------+
| |Seeed XIAO|     | SAMD21 |    ADC    |    DAC   |
+==================+========+===========+==========+
| D0/A0/**DAC**    |  PA2   |   AIN0    | **VOUT** |
+------------------+--------+-----------+----------+
| D1/**A1**        |  PA4   | **AIN4**  |          |
+------------------+--------+-----------+----------+
| D2/**A2**        |  PA10  | **AIN18** |          |
+------------------+--------+-----------+----------+
| **D3**/A3        |  PA11  |   AIN19   |          |
+------------------+--------+-----------+----------+
| D4/A4/**SDA**    |  PA8   |   AIN16   |          |
+------------------+--------+-----------+----------+
| D5/A5/**SCL**    |  PA9   |   AIN17   |          |
+------------------+--------+-----------+----------+
| D6/A6/**TX**     |  PB8   |   AIN2    |          |
+------------------+--------+-----------+----------+
| D7/A7/**RX**     |  PB9   |   AIN3    |          |
+------------------+--------+-----------+----------+
| D8/A8/**SCK**    |  PA7   |   AIN7    |          |
+------------------+--------+-----------+----------+
| D9/A9/**MISO**   |  PA5   |   AIN5    |          |
+------------------+--------+-----------+----------+
| D10/A10/**MOSI** |  PA6   |   AIN6    |          |
+------------------+--------+-----------+----------+

SPI Port
========

The SAMD21 MCU has 6 SERCOM based SPIs. On the Seeed Studio XIAO SAMD21,
SERCOM0 can be put into SPI mode and used to connect to devices over the
|Seeed XIAO| header pin 9 (MISO), pin 10 (MOSI), and pin 8 (SCK).

I2C Port
========

The SAMD21 MCU has 6 SERCOM based USARTs. On the Seeed Studio XIAO SAMD21,
SERCOM2 is available on the |Seeed XIAO| header pin 4 (SDA) and pin 5 (SCL).

Serial Port
===========

The SAMD21 MCU has 6 SERCOM based USARTs. On the Seeed Studio XIAO SAMD21,
SERCOM4 is the Zephyr console and is available on the |Seeed XIAO| header
pins 7 (RX) and 6 (TX).

USB Device Port
===============

The SAMD21 MCU has a (native) USB device port that can be used to communicate
with a host PC. See the :ref:`zephyr:usb-samples` sample applications for more,
such as the :doc:`zephyr:samples/subsys/usb/cdc_acm/README` sample which sets
up a virtual serial port that echos characters back to the host PC. As an
alternative to the default Zephyr console on serial port the Bridle
:ref:`snippet-usb-console` can be used to enable
:ref:`zephyr:usb_device_cdc_acm` and switch the console to USB::

   USB device idVendor=2886, idProduct=802f, bcdDevice= 3.05
   USB device strings: Mfr=1, Product=2, SerialNumber=3
   Product: XIAO SAMD21 (CDC ACM)
   Manufacturer: Seeed Studio
   SerialNumber: AC3FB5052F48A3F7

Programming and Debugging
*************************

The Seeed Studio XIAO SAMD21 ships the BOSSA compatible `UF2 bootloader`_ also
known as `Arduino Zero Bootloader`_, a modern `SAM-BA`_ (Boot Assistant)
replacement. The bootloader can be entered by shorting the RST and GND pads
twice::

   USB device idVendor=2886, idProduct=002f, bcdDevice=42.01
   USB device strings: Mfr=1, Product=2, SerialNumber=3
   Product: Seeeduino XIAO
   Manufacturer: Seeed Studio
   SerialNumber: 2601F57F2E175D24AC3FB5052F48A3F7

Additionally, if :kconfig:option:`CONFIG_USB_CDC_ACM` is enabled then the
bootloader will be entered automatically when you run :code:`west flash`.

.. image:: img/seeed_xiao_samd21_swd.jpg
   :align: right
   :scale: 50%
   :alt: Seeed Studio XIAO SAMD21 (Seeeduino XIAO) SWD Programming Pads

.. tip::

   When ever you need to restore this original bootloader you should read
   and following the directions in `Flashing the Arduino Bootloader using
   DAP Link`_.
   There is also a backup copy of the original bootloader together with
   a ready to use Segger JFlash control file inside the Bridel project:

   * :bridle_file:`boards/arm/seeed_xiao_samd21/doc/bootloader/samd21_sam_ba.hex`
   * :bridle_file:`boards/arm/seeed_xiao_samd21/doc/bootloader/samd21_sam_ba.jflash`

There are also SWD pads on board (PCB bottom side) which have to be
used with tools like Segger J-Link for programming for bootloader restore
or direct programming and debugging.

Flashing
========

#. Build the Zephyr kernel and the :ref:`zephyr:hello_world` sample application:

   .. zephyr-app-commands::
      :zephyr-app: samples/hello_world
      :board: seeed_xiao_samd21
      :goals: build
      :compact:

#. Connect the Seeed Studio XIAO SAMD21 to your host computer using USB.

#. Connect a 3.3 V USB to serial adapter to the board and to the
   host. See the `Serial Port`_ section above for the board's pin
   connections.

#. Run your favorite terminal program to listen for output. Under Linux the
   terminal should be :code:`/dev/ttyUSB0`. For example:

   .. code-block:: console

      $ minicom -D /dev/ttyUSB0 -o

   The -o option tells minicom not to send the modem initialization
   string. Connection should be configured as follows:

   - Speed: 115200
   - Data: 8 bits
   - Parity: None
   - Stop bits: 1

#. Short the RST and GND pads twice quickly to enter bootloader mode.

#. Flash the image:

   .. zephyr-app-commands::
      :zephyr-app: samples/hello_world
      :board: seeed_xiao_samd21
      :goals: flash
      :compact:

   You should see "Hello World! seeed_xiao_samd21" in your terminal.

Debugging
=========

**Debugging is only possible over SWD!**

#. Do the for the debug session necessary steps as before except
   enter the bootloader mode and the flashing.

#. Connect the Segger J-Link to the SWD header (J10).

#. Flash the image and attach a debugger to your board:

   .. zephyr-app-commands::
      :app: zephyr/samples/hello_world
      :board: seeed_xiao_samd21
      :gen-args: -DBOARD_FLASH_RUNNER=openocd
      :goals: debug
      :compact:

   You should ends up in a debug console (e.g. a GDB session).

More Samples
************

LED Blinky
==========

.. zephyr-app-commands::
   :app: zephyr/samples/basic/blinky
   :board: seeed_xiao_samd21
   :goals: flash
   :compact:

LED Fade
========

.. zephyr-app-commands::
   :app: zephyr/samples/basic/fade_led
   :board: seeed_xiao_samd21
   :goals: flash
   :compact:

Basic Threads
=============

.. zephyr-app-commands::
   :app: zephyr/samples/basic/threads
   :board: seeed_xiao_samd21
   :goals: flash
   :compact:

Hello Shell with USB-CDC/ACM Console
====================================

.. zephyr-app-commands::
   :app: bridle/samples/helloshell
   :board: seeed_xiao_samd21
   :west-args: -S usb-console
   :goals: flash
   :compact:

.. rubric:: Simple test execution on target

.. tabs::

   .. group-tab:: Basics

      .. code-block:: console

         uart:~$ hello -h
         hello - say hello
         uart:~$ hello
         Hello from shell.

         uart:~$ hwinfo devid
         Length: 16
         ID: 0x2601f57f2e175d24ac3fb5052f48a3f7

         uart:~$ kernel version
         Zephyr version 3.5.0

         uart:~$ bridle version
         Bridle version 3.5.0

         uart:~$ bridle version long
         Bridle version 3.5.0.0

         uart:~$ bridle info
         Zephyr: 3.5.0
         Bridle: 3.5.0

         uart:~$ device list
         devices:
         - eic@40001800 (READY)
         - gpio@41004480 (READY)
         - gpio@41004400 (READY)
         - cdc-acm-uart-0 (READY)
         - sercom@42001800 (READY)
         - adc@42004000 (READY)
         - dac@42004800 (READY)
         - sercom@42001000 (READY)
         - tcc@42002800 (READY)
         - nvmctrl@41004000 (READY)

         uart:~$ history
         [  0] history
         [  1] device list
         [  2] bridle info
         [  3] bridle version long
         [  4] bridle version
         [  5] kernel version
         [  6] hwinfo devid
         [  7] hello
         [  8] hello -h

   .. group-tab:: GPIO

      Operate with the red Rx user LED:

      .. code-block:: console

         uart:~$ gpio get gpio@41004400 18
         Reading gpio@41004400 pin 18
         Value 0

         uart:~$ gpio conf gpio@41004400 18 out
         Configuring gpio@41004400 pin 18

         uart:~$ gpio set gpio@41004400 18 0
         Writing to gpio@41004400 pin 18

         uart:~$ gpio set gpio@41004400 18 1
         Writing to gpio@41004400 pin 18

         uart:~$ gpio blink gpio@41004400 18
         Blinking port gpio@41004400 index 18. Hit any key to exit

   .. group-tab:: PWM

      Operate with the blue user LED:

      .. code-block:: console

         uart:~$ pwm usec tcc@42002800 1 20000 20000
         uart:~$ pwm usec tcc@42002800 1 20000 19000
         uart:~$ pwm usec tcc@42002800 1 20000 18000
         uart:~$ pwm usec tcc@42002800 1 20000 17000
         uart:~$ pwm usec tcc@42002800 1 20000 16000
         uart:~$ pwm usec tcc@42002800 1 20000 15000
         uart:~$ pwm usec tcc@42002800 1 20000 10000
         uart:~$ pwm usec tcc@42002800 1 20000 5000
         uart:~$ pwm usec tcc@42002800 1 20000 2500
         uart:~$ pwm usec tcc@42002800 1 20000 500
         uart:~$ pwm usec tcc@42002800 1 20000 0

   .. group-tab:: DAC/ADC

      Operate with the loop-back wire from A0 (DAC CH0 VOUT)
      to A1 (ADC CH2 AIN):

     .. code-block:: console

        uart:~$ dac setup dac@42004800 0 10
        uart:~$ adc adc@42004000 resolution 12
        uart:~$ adc adc@42004000 acq_time 10 us
        uart:~$ adc adc@42004000 channel positive 4

        uart:~$ dac write_value dac@42004800 0 512
        uart:~$ adc adc@42004000 read 4
        read: 2028

        uart:~$ dac write_value dac@42004800 0 1023
        uart:~$ adc adc@42004000 read 4
        read: 4054

   .. group-tab:: Flash access

      .. code-block:: console

         uart:~$ flash read nvmctrl@41004000 13630 40
         00013630: 61 6f 5f 73 61 6d 64 32  31 00 48 65 6c 6c 6f 20 |ao_samd2 1.Hello |
         00013640: 57 6f 72 6c 64 21 20 49  27 6d 20 54 48 45 20 53 |World! I 'm THE S|
         00013650: 48 45 4c 4c 20 66 72 6f  6d 20 25 73 0a 00 69 6c |HELL fro m %s..il|
         00013660: 6c 65 67 61 6c 20 6f 70  74 69 6f 6e 20 2d 2d 20 |legal op tion -- |

         uart:~$ flash read nvmctrl@41004000 3c000 40
         0003C000: ff ff ff ff ff ff ff ff  ff ff ff ff ff ff ff ff |........ ........|
         0003C010: ff ff ff ff ff ff ff ff  ff ff ff ff ff ff ff ff |........ ........|
         0003C020: ff ff ff ff ff ff ff ff  ff ff ff ff ff ff ff ff |........ ........|
         0003C030: ff ff ff ff ff ff ff ff  ff ff ff ff ff ff ff ff |........ ........|

         uart:~$ flash test nvmctrl@41004000 3c000 400 2
         Erase OK.
         Write OK.
         Erase OK.
         Write OK.
         Erase-Write test done.

         uart:~$ flash read nvmctrl@41004000 3c000 40
         0003C000: 00 01 02 03 04 05 06 07  08 09 0a 0b 0c 0d 0e 0f |........ ........|
         0003C010: 10 11 12 13 14 15 16 17  18 19 1a 1b 1c 1d 1e 1f |........ ........|
         0003C020: 20 21 22 23 24 25 26 27  28 29 2a 2b 2c 2d 2e 2f | !"#$%&' ()*+,-./|
         0003C030: 30 31 32 33 34 35 36 37  38 39 3a 3b 3c 3d 3e 3f |01234567 89:;<=>?|

         uart:~$ flash page_info 3c000
         Page for address 0x3c000:
         start offset: 0x3c000
         size: 256
         index: 960

         uart:~$ flash erase nvmctrl@41004000 3c000 400
         Erase success.

         uart:~$ flash read nvmctrl@41004000 3c000 40
         0003C000: ff ff ff ff ff ff ff ff  ff ff ff ff ff ff ff ff |........ ........|
         0003C010: ff ff ff ff ff ff ff ff  ff ff ff ff ff ff ff ff |........ ........|
         0003C020: ff ff ff ff ff ff ff ff  ff ff ff ff ff ff ff ff |........ ........|
         0003C030: ff ff ff ff ff ff ff ff  ff ff ff ff ff ff ff ff |........ ........|

   .. group-tab:: I2C

      The Seeed Studio XIAO SAMD21 (Seeeduino XIAO) has no on-board I2C devices.
      For this example the |Grove BMP280 Sensor|_ was connected.

      .. code-block:: console

         uart:~$ log enable none i2c_sam0

         uart:~$ i2c scan sercom@42001000
              0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
         00:             -- -- -- -- -- -- -- -- -- -- -- --
         10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
         20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
         30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
         40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
         50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
         60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
         70: -- -- -- -- -- -- -- 77
         3 devices found on sercom@42001000

         uart:~$ log enable inf i2c_sam0

      The I2C address ``0x77`` is a Bosch BMP280 Air Pressure Sensor and their
      Chip-ID can read from register ``0xd0``. The Chip-ID must be ``0x58``:

      .. code-block:: console

         uart:~$ i2c read_byte sercom@42001000 77 d0
         Output: 0x58

References
**********

.. target-notes::
