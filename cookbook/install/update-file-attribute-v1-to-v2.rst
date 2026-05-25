.. _cookbook_install_update-file-attribute-v1-to-v2:

Updating File Fields When Migrating from MetaModels 1.x to 2.x
===============================================================

Anyone who has not yet completed the migration from Contao 2.x / MetaModels 1.x to
Contao 3.x / MetaModels 2.x will encounter the problem that after a successful update,
embedded images or files are no longer displayed in the frontend. This is because the
corresponding database fields are still of type ``text`` (as used by Contao 2.x / MetaModels
1.x), but must be of type ``blob`` for Contao 3.x / MetaModels 2.x. In addition, file and
folder references stored as plain text must be converted to the corresponding UUIDs.

The following instructions describe how to update file fields that reference either single files
or folders. As an example, we assume an installation with a table **mm_movies** containing two
columns: **image** (single file) and **assets** (folder).

#. Update Contao, e.g. following this guide: |br|
   `Update Contao from 2.11 to 3.5 <https://community.contao.org/de/showthread.php?59748-Update-von-2-11-auf-3-5-Schritt-f%C3%BCr-Schritt>`_
   Make sure that MM tables are not removed during the database update.
#. Update MM: |br|
   First, delete all MM folders under ``/system/modules/``. Then switch the extension
   management to Composer and install the current MM version, e.g. in full via the package
   ``metamodels/bundle_all``. |br|
   After the database update, MetaModels 2.x should be available in the backend as usual.
#. File manager |br|
   If not already done, open the file manager and use the "Synchronise" function to sync the
   existing files with the database.
#. Update attributes |br|
   Open the corresponding file attribute in MetaModels and update/correct the root folder
   setting to match what it was before the update.
#. Create a database backup


Updating Database Fields for Single-File Selections
....................................................

* Open your database in phpMyAdmin or a comparable tool and open the structure view of your
  MetaModel (e.g. mm_movies).
* Create a backup column for the file column using the following SQL statement:
  ``UPDATE mm_movies SET image_backup=image``
* Then change the column type of the file attribute to blob: |br|
  ``ALTER TABLE `mm_movies` CHANGE `image` `image` BLOB NULL DEFAULT NULL``
* Then insert the UUID of the relevant files into the corresponding fields: |br|
  ``UPDATE mm_movies SET image=(SELECT uuid FROM `tl_files` WHERE tl_files.path=mm_movies.image_backup)``
* After the successful update, delete the backup column.


Updating Database Fields for Folder Selections
...............................................

* Open the corresponding file attribute in MetaModels and update/correct the root folder
  setting to match what it was before the update.
* Open your database in phpMyAdmin or a comparable tool and open the structure view of your
  MetaModel. Create a backup column for the file column and copy the column contents into it
  using the following SQL statement: |br|
  ``UPDATE mm_movie SET assets_backup=assets``
* Then change the column type of the file attribute to blob: |br|
  ``ALTER TABLE `mm_movies` CHANGE `assets` `assets` BLOB NULL DEFAULT NULL``
* In the ``backup_assets`` column, find the first fifteen characters (including quotes, up to
  the start of the path to the relevant folder), which look something like:
  ``a:1:{i:0;s:83:"``
* Now adapt the following SQL statement so that the ``CONCAT`` part matches your values: |br|
  ``UPDATE mm_movies SET assets=CONCAT('a:1:{i:0;s:83:"', (`` |br|
  ``SELECT uuid FROM tl_files WHERE path=SUBSTRING(assets_backup, 16, LENGTH(assets_backup)-16-2)), '";}'``  |br|
  ``) WHERE (``  |br|
  ``SELECT uuid FROM tl_files WHERE path=SUBSTRING(assets_backup, 16, LENGTH(assets_backup)-16-2)``  |br|
  ``) IS NOT NULL``
* Afterwards, folder references should work correctly again.


.. |br| raw:: html

   <br />
