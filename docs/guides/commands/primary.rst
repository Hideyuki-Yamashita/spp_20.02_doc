..  SPDX-License-Identifier: BSD-3-Clause
    Copyright(c) 2010-2014 Intel Corporation
    Copyright(c) 2017-2019 Nippon Telegraph and Telephone Corporation


.. _commands_primary:

Primary Commands
================

Primary process is managed with ``pri`` command.

``pri`` command takes a sub command. They must be separated with delimiter
``;``. Some of sub commands take additional arguments.

.. code-block:: console

    spp > pri; SUB_CMD

All of Sub commands are referred with ``help`` command.

.. code-block:: console

    spp > help pri
    Send a command to primary process.

        Show resources and statistics, or clear it.

            spp > pri; status  # show status

            spp > pri; clear   # clear statistics

        Launch secondary process..

            # Launch nfv:1
            spp > pri; launch nfv 1 -l 1,2 -m 512 -- -n 1 -s 192.168....

            # Launch vf:2
            spp > pri; launch vf 2 -l 1,4-7 -m 512 -- --client-id 2 -s ...


.. _commands_primary_status:

status
------

Show status fo ``spp_primary`` and forwarding statistics of each of ports.

.. code-block:: console

    spp > pri; status
    - lcore_ids:
      - master: 0
    - pipes:
      - pipe:0 ring:0 ring:1
    - stats
      - physical ports:
          ID          rx          tx    tx_drop   rxq  txq mac_addr
           0           0           0           0   16   16 3c:fd:fe:b6:c4:28
           1           0           0           0 1024 1024 3c:fd:fe:b6:c4:29
           2           0           0           0    1    1 3c:fd:fe:b6:c4:30
      - ring ports:
          ID          rx          tx     rx_drop     tx_drop
           0           0           0           0           0
           1           0           0           0           0
           2           0           0           0           0
           ...

If you run ``spp_primary`` with forwarder thread, status of the forwarder is
also displayed.

.. code-block:: console

    spp > pri; status
    - lcore_ids:
      - master: 0
      - slave: 1
    - forwarder:
      - status: idling
      - ports:
        - phy:0
        - phy:1
    - pipes:
    - stats
      - physical ports:
          ID          rx          tx    tx_drop  mac_addr
           0           0           0           0  56:48:4f:53:54:00
           1           0           0           0  56:48:4f:53:54:01
      - ring ports:
          ID          rx          tx     rx_drop     tx_drop
           0           0           0           0           0
           1           0           0           0           0
           ...


.. _commands_primary_clear:

clear
-----

Clear statistics.

.. code-block:: console

    spp > pri; clear
    Clear port statistics.


.. _commands_primary_add:

add
---

Add a port with resource ID.

If the type of a port is other than pipe, specify port only.
For example, adding ``ring:0`` by

.. code-block:: console

    spp > pri; add ring:0
    Add ring:0.

Or adding ``vhost:0`` by

.. code-block:: console

    spp > pri; add vhost:0
    Add vhost:0.

If the type of a port is pipe, specify a ring for rx and a ring
for tx following a port. For example,

.. code-block:: console

    spp > pri; add pipe:0 ring:0 ring:1
    Add pipe:0.

.. note::

   pipe is independent of the forwarder and can be added even if the
   forwarder does not exist.

.. _commands_primary_patch:

patch
------

Create a path between two ports, source and destination ports.
This command just creates a path and does not start forwarding.

.. code-block:: console

    spp > pri; patch phy:0 ring:0
    Patch ports (phy:0 -> ring:0).


.. _commands_primary_forward:

forward
-------

Start forwarding.

.. code-block:: console

    spp > pri; forward
    Start forwarding.

Running status is changed from ``idling`` to ``running`` by
executing it.

.. code-block:: console

    spp > pri; status
    - lcore_ids:
      - master: 0
      - slave: 1
    - forwarder:
      - status: running
      - ports:
        - phy:0
        - phy:1
    ...


.. _commands_primary_stop:

stop
----

Stop forwarding.

.. code-block:: console

    spp > pri; stop
    Stop forwarding.

Running status is changed from ``running`` to ``idling`` by
executing it.

.. code-block:: console

    spp > pri; status
    - lcore_ids:
      - master: 0
      - slave: 1
    - forwarder:
      - status: idling
      - ports:
        - phy:0
        - phy:1
    ...


.. _commands_primary_del:

del
---

Delete a port of given resource UID.

.. code-block:: console

    spp > pri; del ring:0
    Delete ring:0.


.. _commands_primary_launch:

launch
------

Launch a secondary process.

Spp_primary is able to launch a secondary process with given type, secondary
ID and options of EAL and application itself. This is a list of supported type
of secondary processes.

  * nfv
  * vf
  * mirror
  * pcap

.. code-block:: console

    # spp_nfv with sec ID 1
    spp > pri; launch nfv 1 -l 1,2 -m 512 -- -n -s 192.168.1.100:6666

    # spp_vf with sec ID 2
    spp > pri; launch vf 2 -l 1,3-5 -m 512 -- --client-id -s 192.168.1.100:6666

You notice that ``--proc-type secondary`` is not given for launching secondary
processes. ``launch`` command adds this option before requesting to launch
the process so that you do not need to input this option by yourself.

``launch`` command supports TAB completion for type, secondary ID and the rest
of options. Some of EAL and application options are just a template, so you
should edit them before launching. Some of default params of options,
for instance, the number of lcores or the amount of memory, are changed from
``config`` command of :ref:`Common Commands<commands_common_config>`.

In terms of log, each of secondary processes are output its log messages to
files under ``log`` directory of project root. The name of log file is defined
with type of process and secondary ID. For instance, ``nfv 2``, the path of log
file is ``log/spp_nfv-2.log``.

.. _commands_primary_flow:

flow
----

Manipulate flow rules.

You can request ``validate`` before creating flow rule.

.. code-block:: console

   spp > pri; flow validate phy:0 ingress group 1 pattern eth dst is
         10:22:33:44:55:66 / vlan vid is 100 / end actions queue index 0 /
         of_pop_vlan / end
   Flow rule validated


You can create rules by using ``create`` request.

.. code-block:: console

   spp > pri; flow create phy:0 ingress group 1 pattern eth dst is
         10:22:33:44:55:66 / vlan vid is 100 / end actions queue index 0 /
         of_pop_vlan / end
   Flow rule #0 created

.. note::

   ``validate`` and/or ``create`` in flow command tends to take long
   parameters. But you should enter it as one line.
   CLI assumes that new line means ``command is entered``. So command
   should be entered without using new line.

You can delete specific flow rule.

.. code-block:: console

   spp > pri; flow destroy phy:0 0
   Flow rule #0 destroyed

Listing flow rules per physical port is supported.

.. code-block:: console

   spp > pri; flow list phy:0
   ID      Group   Prio    Attr    Rule
   0       1       0       -e-     ETH => OF_PUSH_VLAN OF_SET_VLAN_VID OF_SET_VLAN_PCP
   1       1       0       i--     ETH VLAN => QUEUE OF_POP_VLAN
   2       0       0       i--     ETH => JUMP

The following is the parameters to be displayed.

* ``ID``: Identifier of the rule which is unique per physical port.
* ``Group``: Group number the rule belongs.
* ``Prio``: Priority value of the rule.
* ``Attr``: Attributes for the rule which is independent each other.
  The possible values of ``Attr`` are ``i`` or ``e`` or ``t``. ``i`` means
  ingress. ``e`` means egress and ``t`` means transfer.
* ``Rule``: Rule notation.

Flow detail can be listed.

.. code-block:: console

   spp > pri; flow status phy:0 0
   Attribute:
     Group   Priority Ingress Egress Transfer
     1       0        true    false  false
   Patterns:
     - eth:
       - spec:
         - dst: 10:22:33:44:55:66
         - src: 00:00:00:00:00:00
         - type: 0xffff
       - last:
       - mask:
         - dst: ff:ff:ff:ff:ff:ff
         - src: 00:00:00:00:00:00
         - type: 0xffff
     - vlan:
       - spec:
         - tci: 0x0064
         - inner_type: 0x0000
       - last:
       - mask:
         - tci: 0xffff
         - inner_type: 0x0000
   Actions:
     - queue:
       - index: 0
     - of_pop_vlan:
