.. warning:: The information here is not for the current MetaModels 2.2
   but for version 2.1 and older.

.. _cookbook_install_mm2.0-and-older:

MM Installation for 2.1 and Older
===================================


Installing MM 2.1 for Contao 4.4
----------------------------------

The installation requirements for MetaModels 2.1 are:

* a running Contao 4.4.x (LTS) and
* PHP 7.1/7.2
* MySQL from 5.5.5 (InnoDB), MariaDB (without `strict mode`)

Higher versions of Contao and/or PHP are possible but not officially supported.


Installing MetaModels 2.0 for Contao 3.5
------------------------------------------

Installing MetaModels 2.0 for Contao 3 requires a Contao LTS version,
i.e. Contao 3.5.x — as well as the `system requirements equivalent to the
Contao LTS <https://docs.contao.org/books/manual/3.5/de/01-installation/den-live-server-konfigurieren.html>`_.

Since January 2018, MM 2.0 requires at least PHP 5.6.

When upgrading from a "nightly build", it can happen that two tables from the MM Core are
missing and cannot be created by the Contao migration. If this is the case, please create the
following two tables manually:

* tl_metamodel_dcasetting_condition.php
* tl_metamodel_searchable_pages.php


Notes and Instructions for Even Older Contao and MM Versions
-------------------------------------------------------------

:ref:`cookbook_install_update-file-attribute-v1-to-v2`

.. |br| raw:: html

   <br />
