.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../Includes.txt

.. _SQL:

===
SQL
===

Snippets
========

--------------------------
Cleaning up test databases
--------------------------

.. code-block:: sql

    SELECT CONCAT("DROP DATABASE ", SCHEMA_NAME, ";")
    FROM information_schema.SCHEMATA
    WHERE SCHEMA_NAME LIKE "test_%";

