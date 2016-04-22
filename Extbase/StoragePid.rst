.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../Includes.txt

.. _storagePid:

===========================
Mysteries of the StoragePid
===========================

Versions:

    * TYPO3 CMS: 7.6.4
    * Extension builder: 7.6.0
    * Extbase: 7.6.0
    * Example plugion: tx_xxx_pi


Why did you come to this page? Maybe you experienced a szenario similar to
this.

You created an extension with a plugin by using the ``Extension builder``.
You put some entries into a sysfolder and select this folder in the plugins
form as ``Record Storage Page``. In the FE the list the entries show up.

Pretty!

Now you include ``the static template`` into ``TypoScript`` and the entries
**vanish from the list**. You enter the sysfolders ID into the
``Constant Editor`` as ``Default storage PID`` and the entries occur again::

    # Constants
    plugin.tx_xxx_pi.persistence.storagePid = 18

    # Setup
    plugin.tx_xxx_pi.persistence.storagePid = {$plugin.tx_xxx_pi.persistence.storagePid}


Something feels wrong with this static template. It seems the
``Default storage PID`` overrules the settings of the plugin form. Even if
nothing is set, it disables the selection of the plugin form. Shouldn't
a default be a default be a default, that can be overruled by the form?

As we will see, it's not actually the fault of the static template but rather
the algorithm by which ``Extbase`` resolves the storagePid.

A Workaround First
==================

If you are not deeply interested in the sources, here is a workaround.
Disable the ``Default storage PID`` in the ``TypoScript`` setup (and
remove it from the constants)::

    # Setup
    plugin.tx_xxx_pi.persistence.storagePid >

The drawback is, that you loose the option
to set a default, but you can use the settings of the plugin form again.
That is espacially important, if you have multiple forms with different
sysfolders.

The Order of Overrules of the Storage Pid
==========================================

Classes:

    * TYPO3\CMS\Extbase\Configuration\FrontendConfigurationManager
    * TYPO3\CMS\Extbase\Configuration\AbstractConfigurationManager

The class ``FrontendConfigurationManager`` extends the class
``AbstractConfigurationManager``. In ``AbstractConfigurationManager``
the resulution of the configuration is started by the function
``getConfiguration``.

When I speak of the ``storage pid`` here, this may be a list of comma
separated ID as well. However, overrules apply as if it a single value.

The Absolute Default
--------------------

The absolute storage PID default is set as::

    AbstractConfigurationManager::EFAULT_BACKEND_STORAGE_PID = 0

Extbase Settings
----------------

If ``config.tx_extbase.persistence.storagePid`` is set in TypoScript,
this value now overrules the default.


TypoScript Plugin Configuration, First Call
-------------------------------------------

Now ``FrontendConfigurationManager::getPluginConfiguration`` is called
for the first time. A TypoScript setup of the storage pid will overrule
taken from::

    plugin.tx_xxx_pi.persistence.storagePid


Three Steps Called from a sub method
------------------------------------

Next ``FrontendConfigurationManager::getContextSpecificFrameworkConfiguration``
is called, wich itself calls three steps in order.

.. important::

    Here it gets most interesting. Form settings are overruled by plugin TS.

    * Form
    * Plugin TS
    * Flexform

    This explains the unexpected behaviour.

Let's see thee thre steps in details.

Reading the Plugin forms
........................

Here the forms are read by
``FrontendConfigurationManager::overrideStoragePidIfStartingPointIsSet``.


TypoScript Plugin Configuration, Second Call
............................................

``FrontendConfigurationManager::getPluginConfiguration`` is called a second
time and **overrules the settings of the form**.

.. important::

    This not only causes the odd behaviour. It looks strange to call this
    settins a second time at all.

    It's to discuss if this part couldn't be removed from the sources.

Flexform
........

As the last of the three steps flexform settings overrule.

``FrontendConfigurationManager::overrideConfigurationFromFlexForm``

This is the reason why you find the advice in the web to use flexforms as a
workaround to overcome the strange order.

Applying stdWrap
----------------

After all configurations are merged into a common array, stdWrap settings
are evaluated::

    $GLOBALS['TSFE']->cObj->stdWrap($conf['storagePid'], $conf['storagePid.']);

Recursive Folders
-----------------

If the storage folders have subfolders, recursion can now be applied.
It is taken from the setting::

    $conf['persistence']['recursive']

This was basically resolved within the same process as the storage pid and
hence shows the same odd behaviour, that ``TypoSccript`` settings overrule
the form.

The final result is a comma separted list of storage pid.

