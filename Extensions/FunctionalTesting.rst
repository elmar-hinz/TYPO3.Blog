.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../Includes.txt

.. _FunctionalTesting:

======================================
Functional Testing of TYPO3 Extensions
======================================

.. contents::
    :local:
    :backlinks: none


A Minimal Functional Test
=========================

A `TYPO3` **funtional test** extends extends
:code:`\TYPO3\CMS\Core\Tests\FunctionalTestCase`.

.. code-block:: php

    <?php
    namespace ElmarHinz\Ehfaq\Tests\Functional;

    use TYPO3\CMS\Core\Database\DatabaseConnection;

    class HelloWorldTestCase extends \TYPO3\CMS\Core\Tests\FunctionalTestCase
    {

        /**
         * @test
         */
        public function fixtureIsUp()
        {
            $db = $this->getDatabaseConnection();
            $this->assertInstanceOf(DatabaseConnection::class, $db);
        }
    }

    ?>

The functional tests inside an extension are stored in the directory
`Tests/Funtional` or a subdirectory of it.

Running Functional Tests
========================

.. code-block:: bash

    # Running all functional tests of the core from the web root (`app/web/`)
    typo3DatabaseName="test" typo3DatabaseUsername="dev" \
    typo3DatabasePassword="dev" typo3DatabaseHost="127.0.0.1:33333" \
    ../vendor/bin/phpunit -c typo3/sysext/core/Build/FunctionalTests.xml

    # Running all functional tests inside an extension
    typo3DatabaseName="test" typo3DatabaseUsername="dev" \
    typo3DatabasePassword="dev" typo3DatabaseHost="127.0.0.1:33333" \
    ../vendor/bin/phpunit -c typo3/sysext/core/Build/FunctionalTests.xml \
    typo3conf/ext/ehfaq/Tests/Functional/

    # Running one test file inside an extension
    typo3DatabaseName="test" typo3DatabaseUsername="dev" \
    typo3DatabasePassword="dev" typo3DatabaseHost="127.0.0.1:33333" \
    ../vendor/bin/phpunit -c typo3/sysext/core/Build/FunctionalTests.xml \
    typo3conf/ext/ehfaq/Tests/Functional/HelloWorldTestCase.php

The Class Hierarchy
===================

::

    \PHPUnit_Framework_TestCase
          ^
    \TYPO3\CMS\Core\Tests\BaseTestCase
          ^
    \TYPO3\CMS\Core\Tests\FunctionalTestCase
          ^
    \MyTestCase

------------
BaseTestCase
------------

From here on upwards the hierarchy is shard with the `Unit Tests`.
See :ref:`BaseTestCase`.

------------------
FunctionalTestCase
------------------


Source code description
-----------------------

::

    /**
     * Base test case class for functional tests, all TYPO3 CMS
     * functional tests should extend from this class!
     *
     * If functional tests need additional setUp() and tearDown() code,
     * they *must* call parent::setUp() and parent::tearDown() to properly
     * set up and destroy the test system.
     *
     * The functional test system creates a full new TYPO3 CMS instance
     * within typo3temp/ of the base system and the bootstraps this TYPO3 instance.
     * This abstract class takes care of creating this instance with its
     * folder structure and a LocalConfiguration, creates an own database
     * for each test run and imports tables of loaded extensions.
     *
     * Functional tests must be run standalone (calling native phpunit
     * directly) and can not be executed by eg. the ext:phpunit backend module.
     * Additionally, the script must be called from the document root
     * of the instance, otherwise path calculation is not successfully.
     *
     * Call whole functional test suite, example:
     * - cd /var/www/t3master/foo  # Document root of CMS instance, here is index.php of frontend
     * - typo3/../bin/phpunit -c typo3/sysext/core/Build/FunctionalTests.xml
     *
     * Call single test case, example:
     * - cd /var/www/t3master/foo  # Document root of CMS instance, here is index.php of frontend
     * - typo3/../bin/phpunit \
     *     --process-isolation \
     *     --bootstrap typo3/sysext/core/Build/FunctionalTestsBootstrap.php \
     *     typo3/sysext/core/Tests/Functional/DataHandling/DataHandlerTest.php
     */

Variables
---------

:coreExtensionsToLoad:
     If the test case needs additional core extensions as requirement,
     they can be noted here and will be added to LocalConfiguration
     extension list and ext_tables.sql of those extensions will be applied.
     A default list of core extensions is always loaded:
     `core`, `backend`, `frontend`, `lang`, `extbase`, `install`.


:testExtensionsToLoad:
     Array of test/fixture extensions paths that should be loaded for a test.
     Given path is expected to be relative to your document root.

     .. code-block:: php

        [
            'typo3conf/ext/some_extension/Tests/Functional/Fixtures/Extensions/test_extension',
            'typo3conf/ext/base_extension',
         ]

:pathsToLinkInTestInstance:
     Array of test/fixture folder or file paths that should be linked for
     a test. Given paths are expected to be relative to the test instance root.
     The array keys are the source paths and the array values are the
     destination paths.

     .. code-block:: php

        [
            'typo3/sysext/impext/Tests/Functional/Fixtures/Folders/fileadmin/user_upload'
                : 'fileadmin/user_upload',
            'typo3conf/ext/my_own_ext/Tests/Functional/Fixtures/Folders/uploads/tx_myownext'
                : 'uploads/tx_myownext',
        ]

:configurationToUseInTestInstance:
     This configuration array is merged with TYPO3_CONF_VARS
     that are set in default configuration and factory configuration.

:additionalFoldersToCreate:
     Array of folders that should be created inside the test instance document
     root.

     .. code-block:: php

        [ 'fileadmin/user_upload', ]

     Per default the following folder are created::

         /fileadmin
         /typo3temp
         /typo3conf
         /typo3conf/ext
         /uploads

:backendUserFixture:
     The fixture which is used when initializing a backend user.
     Default: ``typo3/sysext/core/Tests/Functional/Fixtures/be_users.xml``

    .. code-block:: xml

        <?xml version="1.0" encoding="utf-8"?>
        <dataset>
                <be_users>
                        <uid>1</uid>
                        <pid>0</pid>
                        <tstamp>1366642540</tstamp>
                        <username>admin</username>
                        <password>$1$tCrlLajZ$C0sikFQQ3SWaFAZ1Me0Z/1</password> <!-- password -->
                        <admin>1</admin>
                        <disable>0</disable>
                        <starttime>0</starttime>
                        <endtime>0</endtime>
                        <options>0</options>
                        <crdate>1366642540</crdate>
                        <cruser_id>0</cruser_id>
                        <workspace_perms>1</workspace_perms>
                        <disableIPlock>1</disableIPlock>
                        <deleted>0</deleted>
                        <TSconfig>NULL</TSconfig>
                        <lastlogin>1371033743</lastlogin>
                        <createdByAction>0</createdByAction>
                        <workspace_id>0</workspace_id>
                        <workspace_preview>1</workspace_preview>
                </be_users>
        </dataset>

Methods
-------

:setup:
    Creates a full test instance `typo3temp/var/tests/functional-[id]`.
    Sets up a database `[prefix]_[id]`. Takes the above settings
    into account.

:getDatabaseConnection:
     Preferrred over $GLOBALS['TYPO3_DB'].

:setUpBackendUserFromFixture:
    This user must exist in the fixture file.

:importDataSet:
    Imports a data set represented as XML into the test database.

:setUpFrontendRootPage:
    Specified by `page id` and `TypoScript` files.

:getFrontendResponse:
    Call the FE output of localhost by `page id` and other optional parameters.

Testing Frontend Output
=======================

--------
The Test
--------

Testing `FE output` requires to fill the tables `pages` and `sys_template`
with a minimum of setup. Afterwards the `FE output` can be tested by the
method `getFrontendResponse`.

.. code-block:: php

    <?php
    namespace ElmarHinz\Ehfaq\Tests\Functional;

    class HelloPageTestCase extends \TYPO3\CMS\Core\Tests\FunctionalTestCase
    {
        protected $testExtensionsToLoad = [ 'typo3conf/ext/ehfaq' ];

        public function setUp()
        {
            parent::setUp();
            $this->importDataSet(
                'typo3/sysext/core/Tests/Functional/Fixtures/pages.xml');
            $this->setUpFrontendRootPage(1,
                  ['EXT:ehfaq/Tests/Functional/Fixtures/Page.ts']);
        }

        /**
         * @test
         */
        public function simplePageOutput()
        {
            $response = $this->getFrontendResponse(1);
            $this->assertContains("<p>Hello world!</p>",
                $response->getContent());
        }

    }

    ?>

While the data into the table `pages` is imported with a general approach
based on XML (`importDataSet`), the data into the table `sys_templates` is
inserted by a dedicated method for `TS` . It doesn't insert the full `TS` but
inserts include links to the `static templates`. The following string
is generated and actually inserted. ``<INCLUDE_TYPOSCRIPT:
source="FILE:typo3conf/ext/ehfaq/Tests/Functional/Fixtures/Page.ts">``.

------------
The Fixtures
------------

A Page Tree as XML
------------------

The core provides a fixture for a small **page tree** in the `XML file`
`typo3/sysext/core/Tests/Functional/Fixtures/pages.xml`.

.. code-block:: xml

    <?xml version="1.0" encoding="utf-8"?>
    <dataset>
        <pages>
            <uid>1</uid>
            <pid>0</pid>
            <title>Root</title>
            <deleted>0</deleted>
            <perms_everybody>15</perms_everybody>
        </pages>

        [ ... shortened ... ]

        <pages>
            <uid>7</uid>
            <pid>0</pid>
            <title>Root 2</title>
            <deleted>0</deleted>
            <perms_everybody>15</perms_everybody>
        </pages>
    </dataset>

A Minimal TypoScript Setup
---------------------------

I put the minimal `TypoScript` setup into the extension into the `TS` file
`EXT:ehfaq/Tests/Functional/Fixtures/Page.ts`.

.. code-block:: ts

    page = PAGE
    page.10 = TEXT
    page.10.value = <p>Hello world!</p>

