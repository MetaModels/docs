.. _rst_cookbook_specials_delete-superfluous-data:

Deleting Superfluous Data
==========================

.. note:: Always create a backup before deleting! - |br|
   e.g. with ``php vendor/bin/contao-console contao:backup:create``

.. tip:: Since MetaModels 2.5, the :ref:`metamodels/health-check <rst_extended_health-check>`
   extension takes over exactly this task directly in the backend - with a preview before
   deletion, a cleanup log and a backup button on the same page, without any shell access
   required, and it covers all the tables listed below. This is the recommended, more
   convenient way; the script here continues to work unchanged though - or use the extension's
   separate commands.

When models or attributes are deleted, it can happen that not all records are
deleted along with them. This affects all attributes that do not store their
data directly in the MetaModel table ``mm_*`` but use their own tables. This is
the case for the following attributes:

* Rating [tl_metamodel_rating]
* Table multi (MCW) [tl_metamodel_tablemulti]
* Text table [tl_metamodel_tabletext]
* Multi-select (tags) [tl_metamodel_tag_relation]
* Translated checkbox [tl_metamodel_translatedcheckbox]
* Translated file [tl_metamodel_translatedlongblob]
* Translated long text [tl_metamodel_translatedlongtext]
* Translated table multi (MCW) [tl_metamodel_translatedtablemulti]
* Translated text table [tl_metamodel_translatedtabletext]
* Translated text [tl_metamodel_translatedtext]
* Translated URL [tl_metamodel_translatedurl]

A :download:`shell script (check-mm-values.sh) for download </_download/check-mm-values.sh>` is
available for displaying and deleting superfluous data.

The script must be placed in the Contao root folder and made executable. Set the
permissions to ``755`` — via an SFTP program or on the console with
``chmod 755 check-mm-values.sh``.

Before use, the PHP path in the configuration section of the file should be
checked and adjusted if necessary.

.. code-block:: shell

   ...
   # Doctrine call (adjust PHP call if necessary)
   DOCTRINE_BIN="php vendor/bin/contao-console doctrine:query:sql"
   ...

On the console, the script can be called with the parameters ``show`` or
``delete`` — ``show`` displays all superfluous data per table and ``delete``
deletes it after confirmation. The call is:

* ``./check-mm-values.sh show`` or
* ``./check-mm-values.sh delete``

Additional tables can be added or removed in the script's configuration section.

The script is written to also run with a simple shell like the one in BusyBox.

.. note:: The check and cleanup may be incorporated into the migration of the
   respective attribute.

.. |br| raw:: html

   <br />
