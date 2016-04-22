.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../../Includes.txt

.. _menus:

=====
Menus
=====

Simple text menu
================

This is the base of a modern CSS styled and JS controlled menu.
It needs to be tweaked to include special functional parts of the
page or to provide HTML classes for special purposes.

.. code-block:: ts

    lib.ehdistribution.mainNavigation = HMENU
    lib.ehdistribution.mainNavigation {
        1 = TMENU
        1 {
            wrap = <ul>|</ul>
            expAll = 1
            NO = 1
            NO {
                wrapItemAndSub = <li>|</li>
                stdWrap.htmlSpecialChars = 1
            }
            CUR < .NO
            CUR.wrapItemAndSub = <li class="current">|</li>
        }
        2 < .1
        3 < .1
    }


Including submenus from libraries
=================================

The content of this site is rendered from multiple Sphinx projects.
Every Sphinx project is exported in JSON format. I use the extension
`restdoc` to convert the JSON files to TYPO3 output.

To load the document structure of a Sphinx project into the TYPO3 main menu
I need to connect both systems at a certain point. This point is the TYPO3
page, where a Sphinx project is loaded and displayed.

Using a `CASE` object is a good approach to include special submenus.
Here the title field of the page is used to detect it.  A page ID is also
a good option. You may use constants to make it fully configurable.

.. code-block:: ts

   NO = 1
    NO {
        wrapItemAndSub = <li>|</li>
        stdWrap.htmlSpecialChars = 1
        stdWrap2.postCObject = CASE
        stdWrap2.postCObject {
            key.field = title
            TYPO3 =< lib.ehdistribution.typo3SphinxSubMenu
            Skills =< lib.ehdistribution.skillsSphinxSubMenu
            ...
        }
    }

The point of transition
-----------------------

The root page of a Sphinx project is connected with a leaf page of
the TYPO3 page tree. That's the point of transition from the DB stored pages
to the JSON stored pages.

Both hiearchies create links. At the point of transition this results in two
alternative links available for the same view. I have to discard one of them.

I decide to discard the root link of the JSON hierarchy and start output at
the first sublevel below it. Both is done within the library
`lib.ehdistribution.typo3SphinxSubMenu`, as one example.

That's the reason for to use `stdWrap2.postCObject`. After the output of the
leaf page of TYPO3 the `postCObject` outputs all subpages of the JSON root
page as `ul`. The hierachy below is nested as usual.

.. code-block:: html

    <ul class="submenu sphinx">
        <li> ... </li>
        <li> ... </li>
    </ul>

