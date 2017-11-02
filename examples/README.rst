Simple Counter Example
======================

To run it using ``docker-compose``:

Open 2 shells, one for Tendermint, and the the other for ABCI.

In the shell for Tendermint:

.. code-block:: bash

    $ docker-compose up tendermint

In the shell for ABCI:

.. code-block:: bash

    $ docker-compose up abci


To play with the counter app you have two options:

1. Use the ``client`` container to send ``curl`` requests.
2. Send ``curl`` requests from the host machine.


Using the ``client`` container to send ``curl`` requests
--------------------------------------------------------

.. code-block:: bash

    $ docker-compose run --rm client /bin/sh
    $ curl http://tendermint:46657/abci_query
    # etc


Sending ``curl`` requests from the host machine
-----------------------------------------------

**Get ip address of container running ``tendermint``**

.. code-block:: bash

    $ docker inspect --format '{{ .NetworkSettings.Networks.pyabci_default.IPAddress }}' `docker ps | grep tendermint | awk '{print $1}'`
    172.18.0.2    # for example



.. code-block:: bash

    $ curl http://172.18.0.2:46657/abci_query

Troubleshooting
---------------
If for some reason, when issuing a ``curl`` request, you get, something like:

.. code-block:: bash

    $ curl http://172.18.0.2:46657/abci_query
    curl: (7) Failed to connect to 172.18.0.2 port 46657: Connection refused

Then you can use the approach of someone who does not really understand how
this works but at least seems to have found that:


.. code-block:: bash

    $ docker-compose run --rm tendermint tendermint unsafe_reset_all

This will delete the ``data/`` directory under ``/tendermint``.
