.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../../Includes.txt

.. _ExtensionFromScratch:

======================
Extension from Scratch
======================

Usually you build an extension with the `Extension Builder`. However like
with every piece of software it is useful do have an idea of the minimal
requirements to get it running. So this chapter is a kind of *Hello World
of Extensions*.

I write this chapter while creating a personal research extension, that I
plan to use in future for all kind of research purposes. I will upload
this extension to TER_ so that people can peep inside. However, it will
**stay alpha forevre**. It is not targeted to be used in production.

I choose the extension key **ehres** for `Elmar Hinz RESearch`.

Registering an Extension Key
============================

Hello World
===========

---------
The Files
---------

This files will go into the minimal extension.

::

    ehres/ext_emconf.php
    ehres/ext_icon.png

That is enough to make the extension installable by the extension mangaer
and be able to upload it to TER_.

-------------------------------
Extension Manager Configuration
-------------------------------

Likely the name of the file `ext_emconf.php` is an abbreviation for
`extension manger configuration`. Here names, dependencies, versions,
and other stuff is configured, so the extension manager, the `TER` and other
programs can read it and take action.

However the future of this file is limited. It will be replaced by the file
`composer.json` sooner or later. To prepare the extension for the composer
installer and autolaoder it is reasonable to provide that file as well,
which causes redundant information in the meantime.


