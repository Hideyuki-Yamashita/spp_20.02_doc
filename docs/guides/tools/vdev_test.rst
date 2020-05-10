..  SPDX-License-Identifier: BSD-3-Clause
    Copyright(c) 2020 Nippon Telegraph and Telephone Corporation

.. _spp_tools_vdev_test:

Vdev_test
=========

Vdev_test is a simple application that it forwards packets received
from rx queue to tx queue on the main core. It can become a secondary
process of the spp_primary. It is mainly used for testing spp_pipe
but it can be used to test any virtual Ethernet devices as well.

Usage
-----

.. code-block:: none

    vdev_test [EAL options] -- [--send] [--create devargs] device-name

Vdev_test runs foreground and stops when Ctrl-C is pressed. If ``--send``
option specified a packet is sent first. The virtual Ethernet device can
be created to specify ``--create`` option.

.. note::

    Since the device can be created by EAL ``--vdev`` option for a
    primary process, ``--create`` option mainly used by a secondary
    process.

Examples
--------

Examining spp_pipe
~~~~~~~~~~~~~~~~~~

.. _figure_vdev_test_example_pipe:

.. figure:: ../images/tools/vdev_test/vdev_test_example_pipe.*
   :width: 50%

It is assumed that pipe ports were created beforehand. First run vdev_test
without ``--send`` option.

.. code-block:: console

    # terminal 1
    $ sudo vdev_test -l 8 -n 4 --proc-type secondary -- spp_pipe0

Then run vdev_test with ``--send`` option on another terminal.

.. code-block:: console

    # terminal 2
    $ sudo vdev_test -l 9 -n 4 --proc-type secondary -- --send spp_pipe1

Press Ctrl-C to stop processes on both terminals after for a while.

Examining vhost
~~~~~~~~~~~~~~~

.. _figure_vdev_test_example_vhost:

.. figure:: ../images/tools/vdev_test/vdev_test_example_vhost.*
   :width: 50%

This example is independent of SPP. First run vdev_test using eth_vhost0
without ``--send`` option.

.. code-block:: console

    # terminal 1
    $ sudo vdev_test -l 8 -n 4 --vdev eht_vhost0,iface=/tmp/sock0,client=1 \
      --file-prefix=app1 -- eth_vhost0

Then run vdev_test using virtio_user0 with ``--send`` option on another
terminal.

.. code-block:: console

    # terminal 1
    $ sudo vdev_test -l 9 -n 4 --vdev virtio_user0,path=/tmp/sock0,server=1 \
      --file-prefix=app2 --single-file-segments -- --send virtio_user0

Press Ctrl-C to stop processes on both terminals after for a while.
