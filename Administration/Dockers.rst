.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../Includes.txt

.. _Dockers:

=======
Dockers
=======

Links
=====

Goals
=====

Conception
==========

Installation of one TYPO3 Docker Boilerplate
============================================

See `TYPO3 Docker Boilerplate` on Github for now.

Maintainance and Backup
=======================

.. _parallelDockers:

Running Multiple Instances in Parallel
======================================

Following the standard way to setup of `TYPO3 Docker Boilerplate`, all
instances will get the same **ports**. The `TYPO3 BE` is accessible by the
*URL* ``http://192.168.56.2:8000/typo3/``.

The drawback is, that only one `TYPO3 Docker suite` can run at a time. I need
to shut down the current one to fire up another. Now one idea of `Dockers`
is, to have a small memory footprint per application. A reason to choose it,
is to be able to run multiple instances in parallel. The trick to this,
is to separate their ports.

The ports are configured in the file ``docker-compose.yml``. I reduce the
`YAML file` to the parts of interest:

.. code::

    app:
        ports:
            - "8000:80"
            - "8443:443"
            - "10022:22"
    mysql:
        ports:
            - "13306:3306"

The last number is the port of the `Docker container`. The first number
is the port on the `Vagrant box` it is mapped to. Now my strategy to separate
the ports, is to add a unique number that identifies each `TYPO3
Docker suite`.

:1: Suite ehfaq
:2: Suite esp
:3: ...

If I assign the number **1** to the `Suite ehfaq` I need to add an offset
of **+1** to all target ports relative to the standard ports.

.. code::

    app:
        ports:
            - "8001:80"
            - "8444:443"
            - "10023:22"
    mysql:
        ports:
            - "13307:3306"

Now I can fire up both in parallel and access each one by a distinct *URL*:

* ``http://192.168.56.2:8000/typo3/``
* ``http://192.168.56.2:8001/typo3/``

The ports don't conflict any more. I keep a central file manually to keep
track of the ID.

