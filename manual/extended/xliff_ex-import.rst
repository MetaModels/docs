.. _rst_extended_xliff_ex-import:

XLIFF Export/Import for MetaModels
====================================

The XLIFF Export/Import tool allows the contents of a Contao installation to be exported for
translation and re-imported. In addition to the standard Contao content, the multilingual
content of MetaModels is also exported and imported.

More on the topic :ref:`Multilingualism in MetaModels <component_multi-language>`.

.. note:: The XLIFF Export/Import tool is still in fundraising and will only be released once
   the target amount of currently 5,397.50 € is reached. |br|
   Early installation via the "Early Adopter Program" is possible — `see below <#rst-extended-xliff-ex-import-early-adopter-program>`_

The export generates an `XLIFF file <https://en.wikipedia.org/wiki/XLIFF>`_ that can be read
by common translation tools — for example `Poedit <https://poedit.net/>`_. XLIFF is the standard
when working with translation agencies.

Once translations have been entered into the exported XLIFF file, it can be re-imported.

Export and import are done via console commands — configuration is done via a YAML file that
you create yourself.

A mapping provider is required for assigning Contao content — currently the
`Changelanguage <https://github.com/terminal42/contao-changelanguage>`_ extension is supported.

The following modules/extensions are currently supported:

* Contao (Core)
* MetaModels (data and backend)
* Isotope 2.x
* RockSolid Custom Elements

More on further plans and development `see below <#rst-extended-xliff-ex-import-extension-possibilities>`_


.. _rst_extended_xliff_ex-import_early-adopter-program:

Early Adopter Program
---------------------

The project is complete at version 1.0 but is not yet freely available. Refinancing is done via
an "Early Adopter Program", meaning you can use the extension immediately upon payment of a
donation. The payment entitles use for one project. Legal claims of any kind are excluded after
payment of a donation.

The amount of the donation should be at least €350*1.

A receipt with VAT stated (or net for EU countries with a valid EU tax ID) will be issued for
contributions. |br|
For interest or further questions, please send an email to info@e-spin.de

*1 Net — plus VAT if applicable.


.. _rst_extended_xliff_ex-import_installation:

Installation via Contao Manager or Composer
-------------------------------------------

Prerequisites for installation:

* MetaModels core 2.1/2.2/2.3/2.4
* Contao 4.4.x/4.9.x/4.13.x/5.3.x


.. _rst_extended_xliff_ex-import_configuration:

Configuration
-------------

After successful installation, the export and import must be configured according to your own
requirements.

First, create a ``/translations`` folder in the Contao installation directory. This is where
exported XLIFF files are stored and from where they are read back during import.

Additionally, a configuration file ``.translation-jobs.yml`` must be created in the project
folder of the Contao installation. This configuration file defines what should be exported or
imported — e.g. only Contao, only MM, or both — and individual jobs are defined here that are
started via console commands.

The configuration file is divided into the sections ``dictionaries`` and ``jobs`` — the
parameters are as follows (`see also example <#rst-extended-xliff-ex-import-example>`_):

.. note:: If the following message appears during `composer update` |br|
   `No default map builder defined, please install an extension that provides "cyberspectrum_i18n.contao.default_map_builder".` |br|
   this indicates that an extension like `Changelanguage <https://github.com/terminal42/contao-changelanguage>`_ or similar is missing

dictionaries
............

* ``*`` Source name: name used in jobs for ``source`` or ``target``; for type ``xliff`` this is the name of the .xlf file
* ``type`` Type: ``contao``, ``metamodels``, ``compound``, ``memory``, or ``xliff``
* ``name`` Name: depends on the type

  * ``contao``: ``contao``
  * ``metamodels``: ``<table_name>``
  * ``compound``: ``*`` freely assignable
  * ``memory``: ``*`` freely assignable
  * ``xliff``: ``*`` freely assignable

Dictionaries of type ``compound`` can in turn contain existing dictionaries and extend them with
additional sources — `see example <#rst-extended-xliff-ex-import-example>`_

jobs
....

* ``*`` Job name: name used in console calls or in other jobs
* ``type`` Type: ``copy`` for copying translation data, or ``batch`` for calling/grouping existing jobs

Type ``copy``:

* ``source``: source name from dictionaries
* ``target``: target name from dictionaries
* ``source_language``: language code e.g. ``en``, ``de`` for the source language
* ``target_language``: language code e.g. ``de``, ``en`` for the target language
* ``copy-source``: defines the behavior when copying from source to target

  * ``true`` (default): source is always copied to target
  * ``if-empty``: source is only copied to target if target is empty or not present
  * ``false``: nothing is copied

* ``copy-target``: defines the behavior when copying from target to source

  * ``true`` (default): target is always copied to source
  * ``if-empty``: target is only copied to source if source is empty or not present
  * ``false``: nothing is copied

* ``remove-obsolete``: defines deletion of a text node

  * ``false`` (default): nothing is deleted
  * ``true``: the text node is deleted when the source is empty or no longer present

* ``filter``: list of RegEx filters on the ``id`` in the ``trans-unit`` node to exclude content

Type ``batch``

* ``jobs``: list of job names to be processed


.. _rst_extended_xliff_ex-import_export:

Export
------

Export is done via a console command with a job name as a parameter — e.g.

``php vendor/bin/contao-console i18n:process export-all -c`pwd`/.translation-jobs.yml``

A single language can also be exported if a corresponding job has been defined — e.g.

``php vendor/bin/contao-console i18n:process export-en-ru -c`pwd`/.translation-jobs.yml``

The ``--help`` parameter outputs all available parameters — e.g. the verbose parameter
(``-v, -vv -vvv``) for more detailed output, or ``--dry-run`` for a "dry run".


.. _rst_extended_xliff_ex-import_import:

Import
------

Import works analogously to export — e.g.

``php vendor/bin/contao-console i18n:process import-all -c`pwd`/.translation-jobs.yml``

or

``php vendor/bin/contao-console i18n:process import-en-ru -c`pwd`/.translation-jobs.yml``


.. _rst_extended_xliff_ex-import_debug:

Debug
-----

It is possible to inspect the translation mapping for problems.
`ChangeLanguage <https://github.com/terminal42/contao-changelanguage>`_ is currently available
as a mapping provider.

For debugging, the command is called with the table of the source language and the target
language as parameters. The ``--help`` parameter outputs help text.

A debug call might look like this:

``php vendor/bin/contao-console debug:i18n-map tl_article.tl_content en de``

This is followed by a tabular listing of the mapping. Where applicable, warnings about problems
are shown beforehand, such as:

.. code-block:: bash

   WARNING   [app] Article 17 (index: 0) has no fallback set, expect problems, I guess it is 13
   ["id" => 17,"index" => 0,"guessed" => 13,"msg_type" => "article_fallback_guess"]


In this case, you should find the article with ID 17 in the backend and check the fallback
article setting.

.. code-block:: bash

   WARNING   [app] Content element 6997 has different type as element in main. Element skipped.
   ["id" => 6997,"mainId" => 7515,"msg_type" => "article_content_type_mismatch"]

The "Change-Language" mapping provider only allows referencing at the page level. Articles and
their content elements are compared based on their order. If there are type differences, the
above message is output; the ID of the CE in the main language would be "7515" here.

If, for example, there are elements in the language to be translated that cannot be found in the
main language, the following message appears:

.. code-block:: bash

   WARNING   [app] Content element 7956 has no mapping in main. Element skipped.
   ["id" => 7956,"msg_type" => "article_content_no_main"]


.. _rst_extended_xliff_ex-import_example:

Example
-------

.. code-block:: yaml
   :linenos:

    dictionaries:
      contao_all:
        type: contao
        name: contao

      combined-content:
        type: compound
        name: content
        dictionaries:
          content: contao_all
          my_staff_export:
            type: metamodels
            name: mm_staff
          # Shorthand version: name as key
          # mm_staff:
          #   type: metamodels
          mm_division:
            type: metamodels
          mm_projects:
            type: metamodels

      mmworkshop:
        type: xliff

    jobs:
      ## Export

      # EN => DE
      export-en-de:
        type: copy
        source: combined-content
        target: mmworkshop
        source_language: en
        target_language: de
        copy-source: true
        copy-target: if-empty
        remove-obsolete: true
        filter:
          - /^content\.tl_article\.[0-9]+\.title$/
          - /^content\.tl_article\.[0-9]+\.alias$/

      # Export all.
      export-all:
        type: batch
        jobs:
          - export-en-de

      ## Import

      # EN => DE
      import-en-de:
        type: copy
        source: mmworkshop
        target: combined-content
        source_language: en
        target_language: de
        copy-source: false
        copy-target: true
        remove-obsolete: false
        filter:
          - /^content\.tl_article\.[0-9]+\.title$/
          - /^content\.tl_article\.[0-9]+\.alias$/

      # Import all.
      import-all:
        type: batch
        jobs:
          - import-en-de

      all:
        type: batch
        jobs:
          - export-all
          - import-all

The dictionaries ``mm_staff``, ``mm_division``, and ``mm_projects`` are the translated
MetaModels — from ``mmworkshop`` the filename ``mmworkshop.xlf`` is derived. Jobs are called
on the console using job names such as ``export-all`` or ``import-all``.

An exported XLIFF file can be opened and edited in an XLIFF editor such as
`Poedit <https://poedit.net/>`_ — see screenshot:

|img_poedit|


.. _rst_extended_xliff_ex-import_extension-possibilities:

Extension Possibilities
-----------------------

Output types

* po
* csv
* xml


.. _rst_extended_xliff_ex-import_donations:

Donations
---------

Thanks for the donations* for the extension to:

* N.N.: 2,700 €
* iMi: 350 €
* Paus medien: 350 €


(Donations are net amounts)


.. |br| raw:: html

   <br />


.. |img_poedit| image:: /_img/screenshots/extended/xliff_ex-import/poedit.png
