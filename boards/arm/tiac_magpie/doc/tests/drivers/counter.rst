.. _tiac_magpie_drivers_counter-tests:

Counter (Real-Time Clock)
#########################

Overview
********

See :zephyr_file:`tests/drivers/counter`
for the original scope of tests, its structure and description.

.. _tiac_magpie_drivers_counter-tests-requirements:

Requirements
************

You will need an ST-LINK/V2 debug tool adapter already connected to the
TiaC Magpie board, which has an already configured UART console connection.

Building and Running
********************

.. tabs::

   .. group-tab:: Running

      Build and run the tests on target as follows:

      .. code-block:: console

         $ west twister \
             --verbose --jobs 4 --inline-logs \
             --enable-size-report --platform-reports \
             --device-testing --hardware-map map.yaml \
             --alt-config-root bridle/zephyr/alt-config \
             --testsuite-root zephyr/tests --tag counter

   .. group-tab:: Results

      You should see the following messages on host console:

      .. parsed-literal::
         :class: highlight

         Device testing on:

         \| Platform    \| ID       \| Serial device   \|
         \|-------------\|----------\|-----------------\|
         \| tiac_magpie \| DT04BNT1 \| /dev/ttyUSB0    \|

         INFO    - JOBS: 4
         INFO    - Adding tasks to the queue...
         INFO    - Added initial list of jobs to queue
         INFO    - 1570/1570 tiac_magpie               tests/drivers/counter/counter_basic_api/drivers.counter.basic_api :bgn:`PASSED` (device: DT04BNT1, 324.902s)

         INFO    - 1782 test scenarios (1570 test instances) selected, 1569 configurations skipped (1569 by static filter, 0 at runtime).
         INFO    - :bgn:`1 of 1570` test configurations passed (100.00%), :bbk:`0` failed, :bbk:`0` errored, :byl:`1569` skipped with :bbk:`0` warnings in :bbk:`358.74 seconds`
         INFO    - In total 10 test cases were executed, 10948 skipped on 1 out of total 638 platforms (0.16%)
         INFO    - :bgn:`1` test configurations executed on platforms, :brd:`0` test configurations were only built.

         Hardware distribution summary:

         \| Board       \| ID       \|   Counter \|
         \|-------------\|----------\|-----------\|
         \| tiac_magpie \| DT04BNT1 \|         1 \|

         INFO    - Saving reports...
         INFO    - Writing JSON report .../twister-out/twister.json
         INFO    - Writing xunit report .../twister-out/twister.xml...
         INFO    - Writing xunit report .../twister-out/twister_report.xml...
         INFO    - Writing target report for tiac_magpie...
         INFO    - Run completed
