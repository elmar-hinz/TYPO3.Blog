.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../Includes.txt

.. _Sphinx authoring system: Sphinx_
.. _git.typo3.org: typo3git_
.. _docs.typo3.org: typo3docs_
.. _Sphinx built in themes: themes_

.. _Documentation:

==================================
Documentation or How to Use Sphinx
==================================

Intro
=====

TYPO3 documentation is written using the `Sphinx authoring system`_. A Sphinx
repositiory consits of multiple related files that are written using the
markup language reStructuredText_. They derive from the programming
language Python_

... and yes, the rendering is done with a Python tool chain,
not with PHP, but there are solutions, to present the Sphinx authored
content within a TYPO3 generated page.

Links
=====

----------------
reStructuredText
----------------

:Introduction: http://docutils.sourceforge.net/docs/user/rst/quickstart.html
:Demo: http://docutils.sourceforge.net/docs/user/rst/demo.html
:Reference: http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html
:Cheatsheet: http://docutils.sourceforge.net/docs/user/rst/cheatsheet.txt

------
Sphinx
------

:Main: http://www.sphinx-doc.org
:Introduction: http://www.sphinx-doc.org/tutorial.html
:Configuration: http://www.sphinx-doc.org/config.html
:Hosting: https://readthedocs.org

Publishing TYPO3 Extension Documentation on Readthedocs
=======================================================

Your documentations gets it's own subdomain like this:

    http://typo3-extension-ehfaq.readthedocs.org

.. tip::
    The subdomain is determined when you create/connect the project by
    the name you specify for it.

---------------------
Why should I do that?
---------------------

Well your documentation is published on https://docs.typo3.org, isn't that
enough? Yes, it is enough. However, publishing to multiple channels may
be better, especially if it can be done by a fully automated process.

-----------
The Concept
-----------

Nowadays there are a lot of services that support continuous integration (CI)
of open sourced software, like the ecosystem around GitHub_. GitHub and
`Read the Docs`_ are well integrated. It's a child game to set up the
triggering mechanisms to automatically render the your documantiation
whenever you push a change.

I use GitHub as an example. Other services `git.typo3.org`_ will provide
similar triggers or you may consider a mirror on Github.

---------
The Setup
---------

The basic setup is easy-peasy. Obviously you need to register an account both
on GitHub_ and `Read the Docs`_. You set up a Git repository for your extension
on `GitHub`, if you didn't already. You fire up the web based administration on
`Read the Docs` to connect it to your GitHub account. Once you have done that
it will present you all your Github repositories. You select those, that
contain a Sphinx documenation that you want to get shown on `Read the Docs`.

That's all to get the triggering set up, but likely it will not successfully
render your docs directly. Depending on your setup you have to do some
tiny adjustments to get it running.

.. tip::
   The main index file of the extension documenation is named Index.rst.
   `Read the Docs` want's it lowercase, beacuse the server is configured
   to ship index.html als the index file. A symblic link does the tric.

   .. code::

       ln -s Index.rst index.rst

.. tip::
    The rendering on `docs.typo3.org`_ requires a file named ``Settings.cfg``
    while `Read the Docs` requires a file named ``conf.py``. A little work,
    but you gain the option to provide different configurations for both.

.. tip::
    See an example_ of conf.py_. Running the sphinx-quickstart_ tool is a
    way to autogenerate that file. (Better use an empty directory.)

.. tip::
    You select the theme by the parameter ``html_theme``. Setting it to
    `default` will give you the ``Read the Docs`` standard theme. Alternatively
    you can selet one of `Sphinx built in themes`_ like `alabaster` or `haiku`.
    (How to set the theme of docs.typo3.org, will require your own research, to
    find out.)
