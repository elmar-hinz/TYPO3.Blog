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

2. Grep for the old version number to find all files to adjust.
   ::

    grep -r '1\.0\.2' .

    ./ChangeLog
    ./Documentation/Settings.yml
    ./ext_emconf.php

3. Adjust the files.

4. Write entry to ChangeLog

5. Commit changes

6. Make tag:
   ::

    git tag 1.0.3
    git push --tags

7. Open upload form:
   ::

    https://typo3.org/extensions/extension-keys/

8. There you find the documentation how to zip with git.
   ::

    git archive -o "${PWD##*/}_x.y.z.zip" HEAD

9. Create file, upload, write upload comment.




