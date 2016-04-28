.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../../Includes.txt

.. _AdvancedFunctionalTesting:

===========================
Advanced Functional Testing
===========================

.. contents::
    :local:
    :backlinks: none

The Basic Workflow
==================

I first manually create the output I want in the usual way. Then I export
a minimalized version of `table data` and `TypoScript setup`. This is bundeled
with the functional test class. Now I can run the tests with different versions
of `PHP` and `TYPO3`.

Exporting XML Datasets to Fixtures
==================================

Tools like `MySQL Workbench` or `Sequel Pro` offer options
to export selected data to XML. This can be used to create `XML` files that
serve as fixtures.

----------
Sequal Pro
----------

First I query the data I want to export by an `SQL query`. Then I click
``Menue > File > Export``. In the popup form I select **XML**,
**Query Results** and **Plain Schema** and the path to save the file to.

Trimming the Output and Including Multiple Templates
====================================================

To focus on the matter of interest, it can get usefull to trim all surrounding
tags, `HTML`, `HEAD`, `BODY`. This is easily done by the `TypoScript`
setting ``config.disableAllHeaderCode``.

Here I include an additional `TypoScript` file `Trim.ts`, that contains just
one line, and test, that the output is actually **equal** to the expectation.

By splitting files this way, I can compose different tests. It shows,
how multiple templates are included. (Just as example. Keep it simple
in practice!)

--------
The Test
--------

.. code-block:: php

    <?php
    namespace ElmarHinz\Ehfaq\Tests\Functional;

    use TYPO3\CMS\Core\Database\DatabaseConnection;

    class TrimPageTest extends \TYPO3\CMS\Core\Tests\FunctionalTestCase
    {

        protected $testExtensionsToLoad = [ 'typo3conf/ext/ehfaq' ];

        public function setUp()
        {
            parent::setUp();
            $this->importDataSet(
                'typo3/sysext/core/Tests/Functional/Fixtures/pages.xml');
            $this->setUpFrontendRootPage(1, [
                'EXT:ehfaq/Tests/Functional/Fixtures/Hello.ts',
                'EXT:ehfaq/Tests/Functional/Fixtures/Trim.ts',
            ]);
        }

        /**
         * @test
         */
        public function trimmedPageOutput()
        {
            $response = $this->getFrontendResponse(1);
            $this->assertEquals("<p>Hello world!</p>",
                $response->getContent());
        }

    }

    ?>

------------
The Fixtures
------------

See :ref:`fixtures`.

The file `Fixtures/Trim.ts` is additionally included.


.. code-block:: typoscript

    config.disableAllHeaderCode = 1


