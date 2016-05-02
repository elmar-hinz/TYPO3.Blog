.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../Includes.txt

.. _cgp:

=============================
Composer - Github - Packagist
=============================

Intro
=====

While `TYPO3` provides it's own mechanism to resolve dependencies via the
`TER`, the more general approach is to host my extension repositories on
`Github` and additionally communicate them via `Packagist`. By this users can
require development versions even before they are published to `TER`.

Overview
========

`Composer` is the dependency manager for PHP. `Git` is a version control system
with support for distributed, non-linear workflows. Both tools are used within
the Continuous Integration workflow of the `TYPO3` project. `Github` connects
public `Git` repositories with a web interface and triggers third party
services. `Packagist` is the central registry to support finding and
autoloading of `Composer` dependencies.

My Hello World Composer Project
===============================

My `Hello World Composer Project` is a directory with two files. There is
the `composer.json` file and a  `.gitignore` to ignore to ignore a possible
`vendor` directory (just for completeness).

.. code-block:: json

    {
        "name": "elmar-hinz/hello-world",
        "description": "Hello World!",
        "license": "MIT",
        "authors": [
            {
                "name": "Elmar Hinz",
                "email": "t3elmar@gmail.com"
            }
        ]
    }

The minimum requirement of a `composer.josn` file are **name** and
**description** accourding to the schema file:

https://github.com/composer/composer/blob/1.1/res/composer-schema.json

.. hint::

    The name is the important part to identify the project. It must be
    lowercase. The first part is the *vendor* name. The recommended choice is
    the username on `Github`. The second part is the name of the package of
    the vendor.

I push it to `Github`.

The head of the master branch will be automatically
accessible by the `Composer` pseudo version **dev-master**. This
is not a real version because the head of the master branch is volatile.

.. code-block:: bash

    git tag 1.0.0
    git push --tags

I create a `Git` tag **1.0.0**. This will automatically become a version by
which `Composer` can select the dependency. In both cases **automatically**
means it is distributed via `Git` and `Packagist` once I have wired up
everything.

.. hint::

    I don't write the version into the `composer.json` file. It will
    automatically be taken form the Git repository.

Finding the Repository
======================

I create a `Hello World` project on `Github`
``https://github.com/elmar-hinz/Playground.HelloComposer``.
The repository URL doesn't need to match the Composer **package** name,
which is ``elmar-hinz/hello-world``. There is no pattern in this, but,
when another package requires my package, it has to know the repository URL
to download it. How?

Either this is written for each package into the requiring `composer.json`.
Alternatively one can ask `Packagist`. `Packagist` acts as a central
repository that delivers the packages by it's name, whereever they are
actually stored, like on `Github` for example.

With this alternative approach the user hasn't to write multiple repositories
into his `composer.json`. `Packagist` is automatically queried by default.
Only a single registration at `Packagist` is required instead.

That the idea behind the `Packagist` repository. Once
registered, it will be triggered from `Github` whenever the `Git`
repository was updated. All new versions are known instantly to the world.


