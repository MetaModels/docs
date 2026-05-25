.. _rst_extended_boilerplate:

MetaModels "Boilerplate"
========================

.. warning:: The information here is outdated and should no longer be used as-is!

The "Boilerplate" extension installs a Contao module for working with MetaModels that contains
various templates for individual customization of MetaModels.

In the boilerplate files, most customizations are commented out and must be "uncommented" as
needed and adapted to the existing MetaModel. The following templates are prepared:

* Custom navigation item for the backend (active)
* Template for a Contao hook (inactive)
* Template for an (MM/DCG) event (inactive)
* Template for default values in the input mask (inactive)


Installing the "Boilerplate" Extension
---------------------------------------

Installation via the extension manager or package manager (Composer) is not possible, as an
update would overwrite your own customizations and settings. For this reason, the extension must
be transferred to the server "manually" via FTP.

The extension can be found on Github at `MetaModels/boilerplate/ <https://github.com/MetaModels/boilerplate/>`_
— see the "Clone or download" button. The extension should be saved locally and transferred to
the server after making your customizations. The "metamodelsboilerplate" folder must be copied
into the "/system/modules/" folder.

The possible customizations are described in the following sections.


Custom Navigation Item for the Backend
---------------------------------------

.. note:: :ref:`rst_cookbook_tips_backend-section`

The basic function of the module — implementing a custom navigation item — is activated. Once the
extension is deployed on the server, a new backend section is available in the input mask
settings under the integration type "Independent" (see screenshot).

|img_backend-integration|

Only when the first MetaModel is assigned to this backend section does the new navigation group
appear in the left navigation.

The name of the navigation group is adjusted in the language files in the "/languages/de" or
"/languages/en" folder in the "modules.php" file. To change the name to "Employee List", the
following entry must be modified:

.. code-block:: php
   :linenos:

   <?php
   /**
    * Custom name of a navigation group in the backend
    */
   $GLOBALS['TL_LANG']['MOD']['metamodelsboilerplate'] = 'Employees';

The position of the new navigation group is determined in the "config.php" file in the "/config"
folder. With the following code:

.. code-block:: php
   :linenos:

   <?php
   /**
    * NAVIGATION
    *
    * Add own navigation group at backend
    * include before e.g. "Design"
    */
   $i = array_search('design', array_keys($GLOBALS['BE_MOD']));
   $GLOBALS['BE_MOD'] = array_merge(array_slice(
       $GLOBALS['BE_MOD'], 0, $i),
       array('metamodelsboilerplate' => array()
       ),
       array_slice($GLOBALS['BE_MOD'], $i)
   );

the navigation group is placed before "design" (labelled "Layout").
The backend navigation could then look as follows:

|img_backend-navigation|

In the backend integration, the MetaModel can also be assigned a custom icon, provided the icon
file is located under "/files/...". A comprehensive icon set is e.g.
`"Fugue Icons" <http://p.yusukekamiyamane.com/>`_.


Template for a Contao Hook
---------------------------

A template for a Contao hook can be found in the "/classes" folder in the file
"MyMetaModelClass.php".

Information about Contao hooks: see `Contao manual <https://docs.contao.org/books/manual/3.4/de/07-contao-anpassen/contao-hooks.html>`_

An example in combination with MetaModels: see :ref:`rst_cookbook_inputmask_regex`


Template for an (MM/DCG) Event
--------------------------------

A template for a Contao hook can be found in the "/config" folder in the file
"event_listeners.php".

An introduction to working with events is e.g.
`"Event-Dispatcher" <https://github.com/contao-community-alliance/event-dispatcher>`_.


Template for Default Values in the Input Mask
----------------------------------------------

A template for default values in the input mask can be found in the "/config" folder in the file
"config.php".

More information at :ref:`rst_cookbook_inputmask_default-values`



.. |img_backend-integration| image:: /_img/screenshots/extended/boilerplate/backend-integration.png
.. |img_backend-navigation| image:: /_img/screenshots/extended/boilerplate/backend-navigation.png

.. |br| raw:: html

   <br />
