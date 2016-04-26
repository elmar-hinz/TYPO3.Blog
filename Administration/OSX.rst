.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../Includes.txt

.. _OSX:

====
OS X
====

Tunneling a MySQL database with SSH
===================================

---------------------------------------------------
In General: Tunneling a remote port to a local port
---------------------------------------------------

Goal
----

I have a `MySQL database` in a remote customer server or a local
`Vagrant box`. I want to access it with my tools as if it is a database
of the local `OS X machine` ``127.0.0.1:33333``. This can be done in a secure
way by tunneling it with `SSH`.

Scenario
--------

Assume the following setup is given
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

:Remote host: 192.168.56.2 (the `Vagrant` box)
:Remote user: vagrant
:Remote password: vagrant
:DB IP inside remote host: 127.0.0.1
:DB port inside remote host: 133006
:DB user: dev
:DB password: dev

Wanted
^^^^^^

:Local IP: 127.0.0.1 (localhost)
:Local Port: 33333

Solution
--------

I dig the `SSH tunnel` like this.

.. code-block:: bash

    ssh -L 127.0.0.1:33333:127.0.0.1:13306 vagrant@192.168.56.2  -N

Now I can access the DB like wanted.

.. code-block:: bash

    mysql --host=127.0.0.1 --port=33333 --user=dev --password=dev

Use ``CTRL-C`` to terminate the tunnel.

.. tip::

    As it is hard to memorize, I also set up an alias.

    .. code-block:: bash

        alias dbtunnel="ssh -L 127.0.0.1:33333:127.0.0.1:13306 vagrant@192.168.56.2  -N"

Explanation
-----------

The parameter :code:`-L` specifies the **link**. The first part is the
**local** one, the second part the **remote**.

The parameter `-N` instructs **not** to open the ssh shell.

Manpage SSH::

 -L      [bind_address:]port:host:hostport
         Specifies that the given port on the local (client) host is to be
         forwarded to the given host and port on the remote side.  This
         works by allocating a socket to listen to port on the local side,
         optionally bound to the specified bind_address.  Whenever a connec-
         tion is made to this port, the connection is forwarded over the
         secure channel, and a connection is made to host port hostport from
         the remote machine.  Port forwardings can also be specified in the
         configuration file.  IPv6 addresses can be specified by enclosing
         the address in square brackets.  Only the superuser can forward
         privileged ports.  By default, the local port is bound in accor-
         dance with the GatewayPorts setting.  However, an explicit
         bind_address may be used to bind the connection to a specific
         address.  The bind_address of ``localhost'' indicates that the lis-
         tening port be bound for local use only, while an empty address or
         `*' indicates that the port should be available from all inter-
         faces.

 -N      Do not execute a remote command.  This is useful for just forward-
         ing ports (protocol version 2 only).

