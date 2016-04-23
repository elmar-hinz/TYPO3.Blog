.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../Includes.txt

.. _ExtensionUpload:

================
Extension upload
================

Manual upload step by step
--------------------------

1. Make Git up-to-date.

#. Grep for the old version number to find all files to adjust.

    .. code::

        grep -r '1\.0\.2' .

        ./ChangeLog
        ./Documentation/Settings.yml
        ./ext_emconf.php

#. Adjust the files.

#. Write entry to ChangeLog

#. Commit and push the changes.

    .. code::

        git commit -am "The message"
        git push

#. Make tag.

    .. code::

        git tag 1.0.3
        git push --tags

#. Zip with git.

    .. code::

        git archive -o "${PWD##*/}_1.0.3.zip" HEAD

    The version of the file must match the version given above.

#. Open upload form.

    .. code::

        https://typo3.org/extensions/extension-keys/

#. Upload with upload comment.


