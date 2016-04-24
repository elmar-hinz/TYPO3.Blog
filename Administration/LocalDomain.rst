.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../Includes.txt

.. _tutorial written by Thomas Sutton: dnsmaqTutorial_


.. _LocalDomain:

=================================================================
Managing a Local Domain dev to Address Multiple Docker Containers
=================================================================

.. contents::
    :local:
    :backlinks: none

Overview
========

I keep my TYPO3 projects in Docker containers on a Macbook Pro.
Dockers run inside Linux. The Linux is a virtual machine controlled
by Vagrant. This results in three nested levels of operating systems:

    OS X -> Ubuntu -> Dockers

Goals
=====

I want to set up a local domain **dev**, to map local subdomains to docker
containers, when called from the web browser::

    elmar.dev => 192.168.56.2:8000
    ehfaq.dev => 192.168.56.2:8001
    esp.dev => 192.168.56.2:8002
    [...]

The IP 192.168.56.2 is my Vagrant controlled virtual Linux machine, that
hosts the docker containers. Each project lives in it's own container and
is accessible by a port on the Vagrant machine.

Conception
==========

The file :code:`/etc/hosts` doesn't work with an asterix with `OS X`. By a file
named :code:`/etc/resolver/dev` I can register the address `127.0.0.1`, the
address of a local DNS, for the top level domain **dev**.

`Dnsmasq` is the choice, as it is easily installed by `Homebrew` and as easily
to configure. In this case all it needs to do, is to serve the address
`127.0.0.1` for all subdomains of **dev**, as we need a proxy for the
next step.

Differnt ports of the Vagrant machine shall serve for different domain
names. This is out of the service of a DNS. A proxy can do this. My choice is
`Haproxy`, again easily to install by `Homebrew` and easily to configure.

Getting the Local DNS Working
=============================

This part is a summary of a `tutorial written by Thomas Sutton`_. Read it
for an extended description.

------------------
Installing Dnsmasq
------------------

I install with Homebrew.

.. code:: bash

    brew update
    brew upgrade
    brew install dnsmasq
    man dnsmaq

The Homebrew installation process outputs some help to get me started. The
actual paths depend on the systems setup:

.. code::

     To configure dnsmasq, copy the example configuration to /Users/ElmarHinz/Homebrew/etc/dnsmasq.conf and edit to taste.
        cp /Users/ElmarHinz/Homebrew/opt/dnsmasq/dnsmasq.conf.example /Users/ElmarHinz/Homebrew/etc/dnsmasq.conf

     To have launchd start dnsmasq at startup:
        sudo cp -fv /Users/ElmarHinz/Homebrew/opt/dnsmasq/*.plist /Library/LaunchDaemons
        sudo chown root /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist

     Then to load dnsmasq now:
        sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist

     WARNING: launchctl will fail when run under tmux.

I follow the advices, how to place the configuration files and how to start
the service. There is only one entry to put into the :code:`dnsmasq.conf` file.

.. code::

    address=/dev/127.0.0.1

Test that the DNS server is working.

.. code::

    dig test.dev @127.0.0.1

The following are to be expected with a working setup.

.. code::

    ;; ANSWER SECTION:
    test.dev.               0       IN      A       127.0.0.1


----------------
Configuring OS X
----------------

The domain **dev** get's a dedicated resolver file in the directory
:code:`/etc/resolver`. If it doesn't already exists, it needs to be created
first. Then the mapping is written into it.

.. code:: bash

    sudo mkdir -p /etc/resolver
    sudo tee /etc/resolver/dev >/dev/null <<EOF
    nameserver 127.0.0.1
    EOF

Check :code:`cat /etc/resolver/dev` answers ``nameserver 127.0.0.1``.

---------------
Testing the DNS
---------------

I can use :code:`ping` to check, if both parts play well together.

.. code:: bash

    # Make sure I haven't brocken my DNS.
    ping -c 1 www.google.com
    # Check that .dev names work
    ping -c 1 test1.dev
    ping -c 1 test2.dev

All :code:`*.dev` domains should now direct to `127.0.0.1`.


Getting the Proxy Working
=========================

------------------
Installing Haproxy
------------------

I install with Homebrew.

.. code:: bash

    brew install haproxy
    man haproxy

I create a configuration file. There is no default path for it.

.. code:: bash

    touch ~/.haproxy.conf

I edit the configuration file to get a minimal response.

.. code::

    defaults
        mode http
        timeout connect 1000ms
        timeout client 50000ms
        timeout server 50000ms

    listen stats
        bind 0.0.0.0:9999
        stats uri /

I start the service for testing.

.. code::

    haproxy -f ~/.haproxy.conf

I visit the URL ``http://localhost:9999`` to the `Statistics Report` running.

Pressing ``CTRL-C`` to stop.

-----------------------
Configuring the Domains
-----------------------

Vagrant and the Dockers are running. Calling
:code:`http://192.168.56.2:8000` displays the local page `Elmar Hinz`.
Now I want to map it to :code:`http://elmar.dev` and similar for other
Dockers.

The minimal configuration looks like this.

.. code::

    global
        daemon

    defaults
        mode http
        timeout connect 1000ms
        timeout client 50000ms
        timeout server 50000ms

    frontend http-in
        bind *:80

        acl is_site1 hdr_end(host) -i elmar.dev
        acl is_site2 hdr_end(host) -i ehfaq.dev
        acl is_site3 hdr_end(host) -i esp.dev

        use_backend elmar if is_site1
        use_backend ehfaq if is_site2
        use_backend esp if is_site3

    backend elmar
        server elmar 192.168.56.2:8000

    backend ehfaq
        server ehfaq 192.168.56.2:8001

    backend esp
        server esp 192.168.56.2:8002

    listen stats
        bind 0.0.0.0:9999
        stats uri /

To make it listen to port **80**, I need to start it as **superuser**.

.. code:: bash

    sudo haproxy -f ~/.haproxy.conf

It is configured as a daemon now and will continue running in the background.
For now I use to commands :code:`ps aux | grep haproxy` and
:code:`sudo kill -9 [process id]` to find and stop it.



