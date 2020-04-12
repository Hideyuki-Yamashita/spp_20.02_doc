..  SPDX-License-Identifier: BSD-3-Clause
    Copyright(c) 2020 Nippon Telegraph and Telephone Corporation


.. _usecase_pipe_pmd:

Pipe PMD
========

Pipe PMD constitutes a virtual Ethernet device (named spp_pipe) using
rings which the spp_primary allocated.

It is necessary for the DPDK application using spp_pipe to implement
it as the secondary process under the spp_primary as the primary
process.

Using spp_pipe enables high-speed packet transfer through rings
among DPDK applications using spp_pipe and SPP secondary processes
such as spp_nfv and spp_vf.

Using pipe PMD
--------------

Create a pipe port by requesting to the spp_primary to use spp_pipe
beforehand.
There are :ref:`CLI<commands_primary_add>` and
:ref:`REST API<api_spp_ctl_spp_primary_put_ports>` to create a pipe
port.
A ring used for rx transfer and a ring used for tx transfer are
specified at a pipe port creation.

For example creating ``pipe:0`` with ``ring:0`` for rx and
``ring:1`` for tx by CLI as follows.

.. code-block:: none

    spp > pri; add pipe:0 ring:0 ring:1

The name as the Ethernet device of ``pipe:N`` is ``spp_pipeN``.
DPDK application which is the secondary process of the spp_primary
can get the port id of the device using ``rte_eth_dev_get_port_by_name``.

Requirement of DPDK application using spp_pipe
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It is necessary to use the common mbuf mempool of the SPP processes.

.. code-block:: C

    #define PKTMBUF_POOL_NAME "Mproc_pktmbuf_pool"

    struct rte_mempool *mbuf_pool;

    mbuf_pool = rte_mempool_lookup(PKTBBUF_POOL_NAME);

Use cases
---------

Here are some examples using spp_pipe.

.. note::

    A ring allocated by the spp_primary assumes it is single
    producer and single consumer. It is user responsibility
    that each ring in the model has single producer and single
    consumer.

Direct communication between applications
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. _figure_pipe_usecase_pipe:

.. figure:: ../images/usecases/pipe_usecase_pipe.*
   :width: 50%

To create pipe ports by CLI before running applications as follows.

.. code-block:: none

    spp > pri; add pipe:0 ring:0 ring:1
    spp > pri; add pipe:1 ring:1 ring:0

Fixed application chain using spp_nfv
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. _figure_pipe_usecase_sfc_nfv:

.. figure:: ../images/usecases/pipe_usecase_sfc_nfv.*
   :width: 50%

To construct the model by CLI before running applications as follows.

.. code-block:: none

    spp > pri; add pipe:0 ring:0 ring:1
    spp > pri; add pipe:1 ring:1 ring:2
    spp > nfv 1; add ring:0
    spp > nfv 1; patch phy:0 ring:0
    spp > nfv 1; forward
    spp > nfv 2; add ring:2
    spp > nfv 2; patch ring:2 phy:1
    spp > nfv 2; forward

Service function chaining using spp_vf
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. _figure_pipe_usecase_sfc_vf:

.. figure:: ../images/usecases/pipe_usecase_sfc_vf.*
   :width: 80%

To construct the model by CLI before running applications as follows.

.. code-block:: none

    spp > pri; add pipe:0 ring:0 ring:1
    spp > pri; add pipe:1 ring:2 ring:3
    spp > pri; add pipe:2 ring:4 ring:5
    spp > vf 1; component start fwd1 2 forward
    spp > vf 1; component start fwd2 3 forward
    spp > vf 1; component start fwd3 4 forward
    spp > vf 1; component start fwd4 5 forward
    spp > vf 1; port add phy:0 rx fwd1
    spp > vf 1; port add ring:0 tx fwd1
    spp > vf 1; port add ring:1 rx fwd2
    spp > vf 1; port add ring:2 tx fwd2
    spp > vf 1; port add ring:3 rx fwd3
    spp > vf 1; port add ring:4 tx fwd3
    spp > vf 1; port add ring:5 rx fwd4
    spp > vf 1; port add phy:1 tx fwd4

Since applications are connected not directly but through spp_vf,
service chaining can be modified without restarting applications.
