.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../Includes.txt

.. _UnitTesting:

=======================
Unit Testing With TYPO3
=======================

Unit tests are stored below :code:`EXT:extkey/Tests/Unit`. A `TYPO3` unit test
extends extends :code:`\TYPO3\CMS\Core\Tests\UnitTestCase`.

:code:`\TYPO3\CMS\Core\Tests\UnitTestCase` extends
:code:`\TYPO3\CMS\Core\Tests\BaseTestCase` extends
:code:`\PHPUnit_Framework_TestCase`.

BaseTestCase
============

`BaseTestCase` provides the following functions.

:getAccessibleMock:
     Creates a mock object which allows for calling protected methods and
     access of protected properties.

:getAccessibleMockForAbstractClass:
     Returns a mock object which allows for calling protected methods and access
     of protected properties. Concrete methods to mock can be specified with
     the last parameter.

:buildAccessibleProxy:
     Creates a proxy class of the specified class which allows
     for calling even protected methods and access of protected properties.
     (Used by `getAccessibleMock` and `getAccessibleMockForAbstractClass`.)

:callInaccessibleMethod:
    Helper function to call protected or private methods.

:inject:
     Injects $dependency into property $name of $target.
     This is a convenience method for setting a protected or private property
     in a test subject for the purpose of injecting a dependency.

:getUniqueId:
     Create and return a unique id optionally prepended by a given string.
     This function is used because on windows and in cygwin environments
     uniqid() has a resolution of one second which results in identical ids
     if simply uniqid('Foo'); is called.

UnitTestCase
============

`UnitTestCase` provides one function `tearDown`.

:tearDown:
     Unset all additional properties of test classes to help PHP
     garbage collection. This reduces memory footprint with lots
     of tests.

.. important::

     If overwriting tearDown() in test classes, please call
     parent::tearDown() at the end. Unsetting of own properties
     is not needed this way.

The property `$testFilesToDelete` is of importance.

:testFilesToDelete:
     Absolute path to files that should be removed after a test.
     Handled in tearDown. Tests can register here to get any files
     within typo3temp/ or typo3conf/ext cleaned up again.
     This is an array.

.. important::

    Register files with `$testFilesToDelete` that are created for testing
    purposes only.

Running Unit Tests
==================

.. code-block:: bash

    # Running all unit tests from the web root (`app/web/`)
    ../vendor/bin/phpunit -c typo3/sysext/core/Build/UnitTests.xml

    # Running all unit tess inside an extension
    ../vendor/bin/phpunit -c typo3/sysext/core/Build/UnitTests.xml typo3conf/ext/ehfaq/Tests/Unit/

    # Running one test file inside an extension
    ../vendor/bin/phpunit -c typo3/sysext/core/Build/UnitTests.xml typo3conf/ext/ehfaq/Tests/Unit/Controller/FAQControllerTest.php



