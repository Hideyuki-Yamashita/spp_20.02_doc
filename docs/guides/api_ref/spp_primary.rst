..  SPDX-License-Identifier: BSD-3-Clause
    Copyright(c) 2018-2019 Nippon Telegraph and Telephone Corporation


.. _spp_ctl_rest_api_spp_primary:

spp_primary
===========

GET /v1/primary/status
----------------------

Show statistical information.

* Normal response codes: 200


Request example
~~~~~~~~~~~~~~~

.. code-block:: console

    $ curl -X GET -H 'application/json' \
      http://127.0.0.1:7777/v1/primary/status


Response
~~~~~~~~

.. _table_spp_ctl_primary_status:

.. table:: Response params of primary status.

    +------------+-------+----------------------------------------+
    | Name       | Type  | Description                            |
    |            |       |                                        |
    +============+=======+========================================+
    | lcores     | array | Array of lcores spp_primary is using.  |
    +------------+-------+----------------------------------------+
    | phy_ports  | array | Array of statistics of physical ports. |
    +------------+-------+----------------------------------------+
    | ring_ports | array | Array of statistics of ring ports.     |
    +------------+-------+----------------------------------------+
    | pipes      | array | Array of pipe ports.                   |
    +------------+-------+----------------------------------------+

Physical port object.

.. _table_spp_ctl_primary_status_phy:

.. table:: Attributes of physical port of primary status.

    +---------+---------+-----------------------------------------------------+
    | Name    | Type    | Description                                         |
    |         |         |                                                     |
    +=========+=========+=====================================================+
    | id      | integer | Port ID of the physical port.                       |
    +---------+---------+-----------------------------------------------------+
    | rx      | integer | The total number of received packets.               |
    +---------+---------+-----------------------------------------------------+
    | tx      | integer | The total number of transferred packets.            |
    +---------+---------+-----------------------------------------------------+
    | tx_drop | integer | The total number of dropped packets of transferred. |
    +---------+---------+-----------------------------------------------------+
    | eth     | string  | MAC address of the port.                            |
    +---------+---------+-----------------------------------------------------+

Ring port object.

.. _table_spp_ctl_primary_status_ring:

.. table:: Attributes of ring port of primary status.

    +---------+---------+-----------------------------------------------------+
    | Name    | Type    | Description                                         |
    |         |         |                                                     |
    +=========+=========+=====================================================+
    | id      | integer | Port ID of the ring port.                           |
    +---------+---------+-----------------------------------------------------+
    | rx      | integer | The total number of received packets.               |
    +---------+---------+-----------------------------------------------------+
    | rx_drop | integer | The total number of dropped packets of received.    |
    +---------+---------+-----------------------------------------------------+
    | tx      | integer | The total number of transferred packets.            |
    +---------+---------+-----------------------------------------------------+
    | tx_drop | integer | The total number of dropped packets of transferred. |
    +---------+---------+-----------------------------------------------------+

Pipe port object.

.. _table_spp_ctl_primary_status_pipe:

.. table:: Attributes of pipe port of primary status.

    +---------+---------+-----------------------------------------------------+
    | Name    | Type    | Description                                         |
    |         |         |                                                     |
    +=========+=========+=====================================================+
    | id      | integer | Port ID of the pipe port.                           |
    +---------+---------+-----------------------------------------------------+
    | rx      | integer | Port ID of the ring port for rx.                    |
    +---------+---------+-----------------------------------------------------+
    | tx      | integer | Port ID of the ring port for tx.                    |
    +---------+---------+-----------------------------------------------------+


Response example
~~~~~~~~~~~~~~~~

.. code-block:: json

    {
      "lcores": [
        0
      ],
      "phy_ports": [
        {
          "id": 0,
          "rx": 0,
          "tx": 0,
          "tx_drop": 0,
          "eth": "56:48:4f:53:54:00"
        },
        {
          "id": 1,
          "rx": 0,
          "tx": 0,
          "tx_drop": 0,
          "eth": "56:48:4f:53:54:01"
        }
      ],
      "ring_ports": [
        {
          "id": 0,
          "rx": 0,
          "rx_drop": 0,
          "tx": 0,
          "tx_drop": 0
        },
        {
          "id": 1,
          "rx": 0,
          "rx_drop": 0,
          "tx": 0,
          "tx_drop": 0
        },
        {
          "id": 2,
          "rx": 0,
          "rx_drop": 0,
          "tx": 0,
          "tx_drop": 0
        }
      ],
      "pipes": [
        {
          "id": 0,
          "rx": 0,
          "tx": 1
        }
      ]
    }


PUT /v1/primary/forward
-----------------------

Start or stop forwarding.

* Normal response codes: 204
* Error response codes: 400, 404


Request example
~~~~~~~~~~~~~~~

.. code-block:: console

    $ curl -X PUT -H 'application/json' -d '{"action": "start"}' \
      http://127.0.0.1:7777/v1/primary/forward


Response
~~~~~~~~

There is no body content for the response of a successful ``PUT`` request.


Equivalent CLI command
~~~~~~~~~~~~~~~~~~~~~~

Action is ``start``.

.. code-block:: none

    spp > pri; forward

Action is ``stop``.

.. code-block:: none

    spp > pri; stop


.. _api_spp_ctl_spp_primary_put_ports:

PUT /v1/primary/ports
---------------------

Add or delete port.

* Normal response codes: 204
* Error response codes: 400, 404


Request (body)
~~~~~~~~~~~~~~

.. _table_spp_ctl_spp_primary_ports_get_body:

.. table:: Request body params of ports of ``spp_primary``.

    +--------+--------+---------------------------------------------------------+
    | Name   | Type   | Description                                             |
    |        |        |                                                         |
    +========+========+=========================================================+
    | action | string | ``add`` or ``del``.                                     |
    +--------+--------+---------------------------------------------------------+
    | port   | string | Resource UID of {port_type}:{port_id}.                  |
    +--------+--------+---------------------------------------------------------+
    | rx     | string | Rx ring for pipe. It is necessary for adding pipe only. |
    +--------+--------+---------------------------------------------------------+
    | tx     | string | Tx ring for pipe. It is necessary for adding pipe only. |
    +--------+--------+---------------------------------------------------------+


Request example
~~~~~~~~~~~~~~~

.. code-block:: console

    $ curl -X PUT -H 'application/json' \
      -d '{"action": "add", "port": "ring:0"}' \
      http://127.0.0.1:7777/v1/primary/ports

For adding pipe.

.. code-block:: console

    $ curl -X PUT -H 'application/json' \
      -d '{"action": "add", "port": "pipe:0", \
      "rx": "ring:0", "tx": "ring:1"}' \
      http://127.0.0.1:7777/v1/primary/ports

Response
~~~~~~~~

There is no body content for the response of a successful ``PUT`` request.


Equivalent CLI command
~~~~~~~~~~~~~~~~~~~~~~

Not supported in SPP CLI.


DELETE /v1/primary/status
-------------------------

Clear statistical information.

* Normal response codes: 204


Request example
~~~~~~~~~~~~~~~

.. code-block:: console

    $ curl -X DELETE -H 'application/json' \
      http://127.0.0.1:7777/v1/primary/status


Response
~~~~~~~~

There is no body content for the response of a successful ``DELETE`` request.


PUT /v1/primary/patches
-----------------------

Add a patch.

* Normal response codes: 204
* Error response codes: 400, 404


Request (body)
~~~~~~~~~~~~~~

.. _table_spp_ctl_spp_primary_ports_patches_body:

.. table:: Request body params of patches of ``spp_primary``.

    +------+--------+------------------------------------+
    | Name | Type   | Description                        |
    |      |        |                                    |
    +======+========+====================================+
    | src  | string | Source port id.                    |
    +------+--------+------------------------------------+
    | dst  | string | Destination port id.               |
    +------+--------+------------------------------------+


Request example
~~~~~~~~~~~~~~~

.. code-block:: console

    $ curl -X PUT -H 'application/json' \
      -d '{"src": "ring:0", "dst": "ring:1"}' \
      http://127.0.0.1:7777/v1/primary/patches


Response
~~~~~~~~

There is no body content for the response of a successful ``PUT`` request.


Equivalent CLI command
~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: none

    spp > pri; patch {src} {dst}


DELETE /v1/primary/patches
--------------------------

Reset patches.

* Normal response codes: 204
* Error response codes: 400, 404


Request example
~~~~~~~~~~~~~~~

.. code-block:: console

    $ curl -X DELETE -H 'application/json' \
      http://127.0.0.1:7777/v1/primary/patches


Response
~~~~~~~~

There is no body content for the response of a successful ``DELETE`` request.


Equivalent CLI command
~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: none

    spp > pri; patch reset


DELETE /v1/primary
------------------

Terminate primary process.

* Normal response codes: 204


Request example
~~~~~~~~~~~~~~~

.. code-block:: console

    $ curl -X DELETE -H 'application/json' \
      http://127.0.0.1:7777/v1/primary


Response
~~~~~~~~

There is no body content for the response of a successful ``DELETE`` request.


PUT /v1/primary/launch
----------------------

Launch a secondary process.

* Normal response codes: 204
* Error response codes: 400, 404


Request (body)
~~~~~~~~~~~~~~

There are four params for launching secondary process. ``eal`` object
contains a set of EAL options, and ``app`` contains options of teh process.

.. _table_spp_ctl_spp_primary_launch_body:

.. table:: Request body params for launch secondary for spp_primary.

    +-----------+---------+-------------------------------------------------+
    | Name      | Type    | Description                                     |
    |           |         |                                                 |
    +===========+=========+=================================================+
    | proc_name | string  | Process name such as ``spp_nfv`` or ``spp_vf``. |
    +-----------+---------+-------------------------------------------------+
    | client_id | integer | Secondary ID for the process.                   |
    +-----------+---------+-------------------------------------------------+
    | eal       | object  | Hash obj of DPDK's EAL options.                 |
    +-----------+---------+-------------------------------------------------+
    | app       | object  | Hash obj of options of secondary process.       |
    +-----------+---------+-------------------------------------------------+

``eal`` object is a hash of EAL options and its values. All of EAL options
are referred in
`EAL parameters
<https://doc.dpdk.org/guides/linux_gsg/linux_eal_parameters.html>`_
in DPDK's
`Getting Started Guide for Linux
<https://doc.dpdk.org/guides/linux_gsg/index.html>`_.

``app`` object is a hash of options of secondary process, and you can refer
options of each of processes in
`How to Use
<https://spp-tmp.readthedocs.io/en/latest/setup/howto_use.html>`_
section.


Request example
~~~~~~~~~~~~~~~

Launch ``spp_nfv`` with secondary ID 1 and lcores ``1,2``.

.. code-block:: console

    $ curl -X PUT -H 'application/json' \
      -d "{'proc_name': 'spp_nfv', 'client_id': '1', \
        'eal': {'-m': '512', '-l': '1,2', '--proc-type': 'secondary'}, \
        'app': {'-s': '192.168.11.59:6666', '-n': '1'}}"
      http://127.0.0.1:7777/v1/primary/launch

Launch ``spp_vf`` with secondary ID 2 and lcores ``1,4-7``.

.. code-block:: console

    $ curl -X PUT -H 'application/json' \
      -d "{'proc_name': 'spp_vf', 'client_id': '2', \
        'eal': {'-m': '512', '-l': '1,4-7', '--proc-type': 'secondary'}, \
        'app': {'-s': '192.168.11.59:6666', '--client-id': '2'}}"
      http://127.0.0.1:7777/v1/primary/launch


Response
~~~~~~~~

There is no body content for the response of a successful ``PUT`` request.


Equivalent CLI command
~~~~~~~~~~~~~~~~~~~~~~

``proc_type`` is ``nfv``, ``vf`` or ``mirror`` or so.
``eal_opts`` and ``app_opts`` are the same as launching from command line.

.. code-block:: none

    pri; launch {proc_type} {sec_id} {eal_opts} -- {app_opts}

POST /v1/primary/flow_rules/port_id/{port_id}/validate
------------------------------------------------------

Validate flow rule for specific port_id.

* Normal response codes: 200


Request example
~~~~~~~~~~~~~~~

.. code-block:: console

    $ curl -X POST \
           http://127.0.0.1:7777/v1/primary/flow_rules/port_id/0/validate \
           -H "Content-type: application/json" \
           -d '{ \
               "rule": \
                  { \
                  "group": 0, \
                  "priority": 0, \
                  "direction": "ingress", \
                  "transfer": true, \
                  "pattern": \
                    [ \
                      "eth dst is 11:22:33:44:55:66 type mask 0xffff", \
                      "vlan vid is 100" \
                    ], \
                  "actions": \
                    [ \
                      "queue index 1", \
                      "of_pop_vlan" \
                    ] \
                  } \
               }'

Response
~~~~~~~~

.. _table_spp_ctl_primary_flow_validate:

.. table:: Response params of validate.

    +------------+--------+----------------------------------------+
    | Name       | Type   | Description                            |
    |            |        |                                        |
    +============+========+========================================+
    | result     | string | Validation result.                     |
    +------------+-------++----------------------------------------+
    | message    | string | Additional information if any.         |
    +------------+-------++----------------------------------------+

Response example
~~~~~~~~~~~~~~~~

.. code-block:: json

        {
                "result" : "success",
                "message" : "Flow rule validated"
        }

POST /v1/primary/flow_rules/port_id/{port_id}
---------------------------------------------

Create flow rule for specific port_id.

* Normal response codes: 200


Request example
~~~~~~~~~~~~~~~

.. code-block:: console

    $ curl -X POST http://127.0.0.1:7777/v1/primary/flow_rules/port_id/0 \
           -H "Content-type: application/json" \
           -d '{ \
               "rule": \
                  { \
                  "group": 0, \
                  "priority": 0, \
                  "direction": "ingress", \
                  "transfer": true, \
                  "pattern": \
                    [ \
                      "eth dst is 11:22:33:44:55:66 type mask 0xffff", \
                      "vlan vid is 100" \
                    ], \
                  "actions": \
                    [ \
                      "queue index 1", \
                      "of_pop_vlan" \
                    ] \
                  } \
               }'

Response
~~~~~~~~

.. _table_spp_ctl_primary_flow_create:

.. table:: Response params of flow creation.

    +------------+--------+----------------------------------------+
    | Name       | Type   | Description                            |
    |            |        |                                        |
    +============+========+========================================+
    | result     | string | Creation result.                       |
    +------------+--------+----------------------------------------+
    | message    | string | Additional information if any.         |
    +------------+--------+----------------------------------------+
    | rule_id    | string | Rule id allocated if successful.       |
    +------------+--------+----------------------------------------+

Response example
~~~~~~~~~~~~~~~~

.. code-block:: json

        {
                "result" : "success",
                "message" : "Flow rule #0 created",
                "rule_id" : "0"
        }

DELETE /v1/primary/flow_rule/port_id/{port_id}
----------------------------------------------

Delete all flow rule for specific port_id.

* Normal response codes: 200


Request example
~~~~~~~~~~~~~~~

.. code-block:: console

    $ curl -X DELETE http://127.0.0.1:7777/v1/primary/flow_rule/port_id/0

Response
~~~~~~~~

.. _table_spp_ctl_primary_flow_flush:

.. table:: Response params of flow flush.

    +------------+--------+----------------------------------------+
    | Name       | Type   | Description                            |
    |            |        |                                        |
    +============+========+========================================+
    | result     | string | Deletion result.                       |
    +------------+--------+----------------------------------------+
    | message    | string | Additional information if any.         |
    +------------+--------+----------------------------------------+

Response example
~~~~~~~~~~~~~~~~

.. code-block:: json

        {
                "result" : "success",
                "message" : "Flow rule all destroyed"
        }

DELETE /v1/primary/flow_rule/{rule_id}/port_id/{port_id}
--------------------------------------------------------

Delete specific flow rule for specific port_id.

* Normal response codes: 200


Request example
~~~~~~~~~~~~~~~

.. code-block:: console

    $ curl -X DELETE http://127.0.0.1:7777/v1/primary/flow_rules/0/port_id/0

Response
~~~~~~~~

.. _table_spp_ctl_primary_flow_delete:

.. table:: Response params of flow deletion.

    +------------+--------+----------------------------------------+
    | Name       | Type   | Description                            |
    |            |        |                                        |
    +============+========+========================================+
    | result     | string | Deletion result.                       |
    +------------+--------+----------------------------------------+
    | message    | string | Additional information if any.         |
    +------------+--------+----------------------------------------+

Response example
~~~~~~~~~~~~~~~~

.. code-block:: json

        {
                "result" : "success",
                "message" : "Flow rule #0 destroyed"
        }
