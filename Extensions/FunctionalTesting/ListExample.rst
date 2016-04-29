.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../../Includes.txt

.. _FunctionalTestingListExample:

==================================
List Example of Functional Testing
==================================

.. contents::
    :local:
    :backlinks: none

The is a real world example of a funtional test. It tests the expected
output of a list of FAQ. It shows, that sorting works as expected.

For reasons of simplicity everthing is set up on a single page,
the *TS root template*, the *datasets*, the *HTML output*.

--------
The Test
--------

.. code-block:: php

    <?php
    namespace ElmarHinz\Ehfaq\Tests\Functional;

    class ListTest extends \TYPO3\CMS\Core\Tests\FunctionalTestCase
    {
        protected $testExtensionsToLoad = [ 'typo3conf/ext/ehfaq' ];

        public function setUp()
        {
            parent::setUp();
            $this->importDataSet(
                'typo3conf/ext/ehfaq/Tests/Functional/Fixtures/List.xml');
            $this->setUpFrontendRootPage(1, [
                'EXT:ehfaq/Tests/Functional/Fixtures/List.ts',
                'EXT:ehfaq/Tests/Functional/Fixtures/Trim.ts',
            ]);
        }

        /**
         * @test
         */
        public function listOutput()
        {
            $expected = <<< HTML
        <div class="tx-ehfaq">
            <section class="tx-ehfaq-topic">
                <header class="tx-ehfaq-topic-header"><h1>Question 1</h1></header>
                <div class="tx-ehfaq-topic-body ce-intext"> Answer 1 </div>
            </section>
            <section class="tx-ehfaq-topic">
                <header class="tx-ehfaq-topic-header"><h1>Question 2</h1></header>
                <div class="tx-ehfaq-topic-body ce-intext"> Answer 2 </div>
            </section>
            <section class="tx-ehfaq-topic">
                <header class="tx-ehfaq-topic-header"><h1>Question 3</h1></header>
                <div class="tx-ehfaq-topic-body ce-intext"> Answer 3 </div>
            </section>
        </div>
    HTML;
            $expected = $this->tidy($expected);
            $response = $this->getFrontendResponse(1);
            $actual = $this->tidy($response->getContent());
            $this->assertEquals($expected, $actual);
        }

        /**
         * Tidy string
         *
         * Trims the string and reduces all whitespace
         * to a single blank.
         *
         * @param string dirty
         * @return string cleaned
         */
        private function tidy($str)
        {
            return trim(preg_replace('/\s\s+/', ' ', $str));
        }

    }

    ?>

A special challange is, that the output rendering of `RTE` fields creates
a lot of whitespace, that is ugly to insert as expectation. I apply a minimal
*tidy function* before I compare the expected and the actual output string.

The whitespace is of little interest, but I check the order of the tags, the
classes and the content strings.

.. hint::

    If the tags would contain multiple attributes, that could appear in
    arbitrary order, other forms of testing need to be considered, that
    include parsing into and comparing of DOM.

------------
The Fixtures
------------

The DB Fixture
--------------

The order of the `tx_ehfaq_domain_model_faq` entries as well as it's `uid`
differ from the order of the field `sorting`. By this it can be shown, that the
order of output is determined by the field `sorting`.

.. code-block:: xml

    <?xml version="1.0" encoding="utf-8"?>

    <dataset>

        <pages>
            <uid>1</uid>
            <pid>0</pid>
            <title>FAQ</title>
        </pages>

        <tx_ehfaq_domain_model_faq>
            <uid>1</uid>
            <pid>1</pid>
            <sorting>128</sorting>
            <question>Question 2</question>
            <answer>Answer 2</answer>
        </tx_ehfaq_domain_model_faq>

        <tx_ehfaq_domain_model_faq>
            <uid>2</uid>
            <pid>1</pid>
            <sorting>256</sorting>
            <question>Question 3</question>
            <answer>Answer 3</answer>
        </tx_ehfaq_domain_model_faq>

        <tx_ehfaq_domain_model_faq>
            <uid>3</uid>
            <pid>1</pid>
            <sorting>64</sorting>
            <question>Question 1</question>
            <answer>Answer 1</answer>
        </tx_ehfaq_domain_model_faq>

    </dataset>

The TypoScript Fixture
----------------------

It is quite difficult to test from the point of configuration of a plugin
into `tt_content``. This would include a lot of ``TypooScript`` templates
including constants. This would blow up the test and I don't know a test case
base class, that would prepare this setup as a default.

However, it is easy to test from the point of the call to
``TYPO3\CMS\Extbase\Core\Bootstrap->run``.

.. code-block:: none

    page = PAGE
    page.10 = USER
    page.10 {
        userFunc = TYPO3\CMS\Extbase\Core\Bootstrap->run
        extensionName = Ehfaq
        pluginName = Faq
        vendorName = ElmarHinz
        persistence.storagePid = 1
    }


