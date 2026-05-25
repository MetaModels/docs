.. _component_translations:

Symfony Translation
===================

.. note:: Symfony translation is implemented from MetaModels version 2.3.

.. _component_translations_info:
Quick Info
----------

The application outputs of MetaModels in Contao are provided in multiple languages. This refers to
all navigation outputs, labels and descriptions of input widgets, legends of input forms, etc. —
but not the entered "payload data". From MetaModels version 2.3, the provision is handled by
`Symfony Translation <https://symfony.com/doc/current/translation.html>`_, which is characterized
by various advantages such as excellent caching.

To load new or changed entries, the Symfony translation cache must be cleared. This can be done
via the console or via the manager.

.. note:: Clearing the Symfony translation cache can also be done in the backend — see
   Settings > System maintenance > Symfony translator |br|
   Note: here only the cache of the currently set working environment (prod or dev) is cleared!

The translations themselves are still managed via the `Transifex <https://app.transifex.com/metamodels/>`_
website. Anyone can participate in translating our base language English into other languages via
Transifex.

Existing custom translation texts that were created as PHP arrays must be transferred to an XLIFF
file (see "Custom translation adjustments").

Background
----------

In Contao and also MetaModels, more and more native Symfony components are being used and existing
custom developments substituted. In MetaModels version 2.3, the translation was largely converted to
the Symfony component `Translation <https://symfony.com/doc/current/translation.html>`_ and the
translations are stored directly in XLIFF format.

Symfony Translation provides very good caching of translated texts, thereby speeding up the backend
build. This cache is generated once when Contao starts — if not already present — and is then
available for all calls.

MetaModels has a special feature compared to other extensions: in MetaModels, inputs are also made
that affect the backend display — e.g. the models in the main navigation, names and descriptions
from attributes for input widgets, etc. This means that for new entries or changes to texts, the
translation cache must be rebuilt. Rebuilding is forced by clearing the cache — this can be done
via the Contao Manager or via the console.

The cache is typically located in the folders

- var/cache/dev/translations
- var/cache/prod/translations

The XLIFF files are now in the MM repositories in the folder ``Resources/translations/`` — a few
translations that are passed directly to Contao had to remain in ``Resources/contao/languages``.

Translations can now easily be tracked via the Symfony toolbar. In the "Translation" panel, information
about found and not found translations as well as fallbacks is listed.


Custom Translation Adjustments
--------------------------------

New overrides must be created as an XLIFF file. The structure can be seen in the file whose value
you want to change. Note that XLIFF files in DCG are in version 2.0 and in MetaModels in version 1.2
— the structure differs somewhat.

Example: to rename the "Filter" button to "Search" for German, a file must be created as

- translations/metamodels_filter.de.xlf or
- src/Resources/translations/metamodels_filter.de.xlf

with the content

.. code-block:: xliff
   :linenos:

   <?xml version="1.0" ?>
   <xliff xmlns="urn:oasis:names:tc:xliff:document:1.2" version="1.2">
     <file source-language="en" datatype="plaintext" original="src/CoreBundle/Resources/translations/metamodels_filter.en.xlf" target-language="de">
       <body>
         <trans-unit id="submit" resname="submit">
           <source>Filter</source>
           <target>Search</target>
         </trans-unit>
       </body>
     </file>
   </xliff>

Don't forget to clear the translation cache after applying!

.. _component_translations_lns:
Message "LABEL NOT SET"
------------------------

If the message "LABEL NOT SET" is displayed as a label in the input form, there can be several
reasons:

The simplest reason is that something has changed on the label but the cache has not been renewed —
please clear it (:ref:`see above <component_translations_info>`).

If you have made custom DCA adjustments to input widgets, e.g. for default values or to embed a
custom wizard icon, Contao "magic" unfortunately kicks in and tries to add the label from the
translations array — but these no longer exist with Symfony Translations.

The message is easy to fix by additionally adding a label in the DCA file — the value can be empty,
e.g. for MM "mm_employees" and attribute "name":

.. code-block:: php
   :linenos:

   // src/Resources/contao/dca/mm_employees.php or contao/dca/mm_employees.php

   // Add label to fix Contao "magic add".
   $GLOBALS['TL_DCA']['mm_employees']['fields']['name']['label'] = '';

.. |br| raw:: html

   <br />
