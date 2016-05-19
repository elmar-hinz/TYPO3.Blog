.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../../Includes.txt

.. _FunctionalTestingDebugging:

==================================
Debugging while Functional Testing
==================================

.. contents::
    :local:
    :backlinks: none

The Issue
=========

The system under test is running in it's very own process. The interface
between both processes is handled by PHPUnit. It is complex, because a
full call via `HTTP` is constructed and answered, stuff usually managed by
a web-server, encluding the setup of the full environment.

Unfortunatly the error feedback between these processes doesn't work as
one would wish. Whenever the subprocess is breaking, the error
feedback to the calling unit test is also broken. You become blind.

Exceptions on higher levels are reported, as long as they don't break
the script.


The Solution
=============

The solution is, to write the errors to a logfile. `TYPO3` has it's own
execption and error handling system, but I didn't analyse how it behaves
for the child process of a functional test setup.

Instead I register an error handler right in the place I am working on.
Without filtering the loglevel it may report an exessive amount of
`E_NOTICE`. It worked for me to log those messages lower than
`E_NOTICE` only.







