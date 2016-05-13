.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../../Includes.txt

.. _FunctionalTestingWorkflow:

==================================
The Workflow of Functional Testing
==================================

.. contents::
    :local:
    :backlinks: none

The Basic Workflow
==================

I first manually create the output I want in the usual way. This gives me
a working DB and output that I can control visually. Now this needs to be
packed into a test that I can run with different versions of PHP and TYPO3.
A good solution to run this test triggered by each `GIT` commit is the
combination of `Github` and `Travis`.

I export a minimalized version of table data and `TypoScript` setup. This is
bundeled with the functional test class. Export means, I either create the
`XML` files by hand or I actually export a selection of the data in the
database as `XML`.

The TYPO3 XML DB Fixture Format
===============================

The examples will show how the `XML` should be structured that the `TYPO3`
functional testing expects. It usually differs a little from the `XML`
that is exported by the tools and needs to be adjusted accordingly.

The enclosing tag is ``dataset``. The children have the names of the
tables. That is one tag per dataset, not per table. The tags of the
sublevel name the fields.

.. code-block:: xml

    <?xml version="1.0" encoding="utf-8"?>
    <dataset>

        <pages>
            <uid>1</uid>
            <pid>0</pid>
            [ ... more fields here ... ]
        </pages>

        <pages>
            <uid>1</uid>
            <pid>0</pid>
            [ ... more fields here ... ]
        </pages>

        <tt_content>
            <uid>1</uid>
            <pid>0</pid>
            [ ... more fields here ... ]
        </tt_content>

        [ ... more datasets here ... ]

    </dataset>

Size of the Fixtures
====================

I try to keep the fixtures as small is reasonable. In general I reduce the
exports to the fields of interest and trust that the DB is set up well enough,
to set the other fields to reasonable default values. Giving up the full
control over the fixture is a price I pay, to gain more speed in writing tests,
better readability and lower costs of maintenance.

If bugs are observed, it becomes necessary to write tests with a more
detailed control of the fixture.

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


.. code-block:: none

    config.disableAllHeaderCode = 1

