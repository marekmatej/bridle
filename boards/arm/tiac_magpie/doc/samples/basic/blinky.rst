.. _tiac_magpie_led_blinky-sample:

LED Blinky
##########

Overview
********

See :doc:`zephyr:samples/basic/blinky/README` for the original description.

.. _tiac_magpie_led_blinky-sample-requirements:

Requirements
************

The TiaC Magpie board have two "User LEDs" connected via GPIO pins. The first
LED is configured using the ``led0`` :ref:`devicetree <zephyr:dt-guide>` alias.

Building and Running
********************

Build and flash LED Blinky as follows:

.. zephyr-app-commands::
   :app: zephyr/samples/basic/blinky
   :build-dir: led_blinky-tiac_magpie
   :board: tiac_magpie
   :west-args: -p
   :goals: build flash
   :host-os: unix

After flashing, the first "User LED" starts to blink.
LED Blinky does not print to the console.
