.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../Includes.txt

.. _Git:

===
Git
===

Tags
====

Create:

.. code-block:: bash

    # local
    git tag 1.1.0
    git tag 1.1.1 -m "With a commit message"
    git tag 1.0.9 b854e3877e2e2bf768c4f04acfd72fda94dbbb40

    # push to remote
    git push --tags

List:

.. code-block:: bash

    # local
    git tag
    git tag -l
    git tag -l 1.*

    #remote
    git ls-remote --tags

Delete:

.. code-block:: bash

    # local
    git tag -d 1.0.0

    # remote
    git push --delete origin 1.0.0


