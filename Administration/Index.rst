.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../Includes.txt

.. _Admin:

==============
Administration
==============

Overview
========

I keep my `TYPO3` projects in `Docker` containers on a `Macbook Pro`.
`Docker` containers live inside a `Linux` machine. My `Linux`
is a virtual machine controlled by `Vagrant`, also addressed as
**Vagrant box**. This setup results in three nested OS levels:

    OS X -> Ubuntu -> Dockers

The `Docker` containers are mapped to **ports** of the `Vagrant` box.

The following articles will describe my `OS X` setup with `Homebrew` and
`Ansible`, the `Vagrant` box, the `Dockers` setup and the management
of a local `DNS` system to map local domains to the `Docker` containers.
It is work in progress.

Index
=====

.. toctree::
    OS X <OSX>
    Vagrant Controlled Development Machine (Linux) <DevelopmentMachine>
    Docker Containers for TYPO3 Projects <Dockers>
    Managing a Local Domain: dev<LocalDomain>
