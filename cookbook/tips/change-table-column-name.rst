.. _rst_cookbook_tips_change_table_column_name:

Renaming Tables or Columns
===========================

.. note:: The schema manager is available from version 2.3 — :ref:`see Schema Manager <component_schema-manager>`

Table and column names should be chosen carefully, as renaming them after the fact is a
rather critical operation and should be avoided if possible.

However, renaming a table or column is possible. With the schema manager, the database
adjustment task is delegated to Doctrine, which performs it as part of a database migration.
In Doctrine, renaming a table or column does not simply overwrite the names — instead, the new
element is created and the old one is deleted.

If the table or column contains no user data yet, the adjustment can be made without issue.

If user data already exists and must be preserved, manual intervention is required to back up
the data during the adjustment. An advantage of the schema manager is that the two operations
(Create and Drop) happen in sequence, giving you the opportunity to transfer data from the old
to the new element in between.

For columns, this only applies to attributes that create their own column in the ``mm_*`` table,
such as Text or Alias (simple attributes).

This transfer can be done using standard database tools like phpMyAdmin or directly on the
command line. After the new table or column has been created, data can be copied with the
following commands:

Table: |br|
``php vendor/bin/contao-console doctrine:query:sql 'INSERT mm_test_new SELECT * FROM mm_test_old'``

Column: |br|
``php vendor/bin/contao-console doctrine:query:sql 'UPDATE mm_test SET col_new=col_old'``

Note: to start the migration: |br|
``php vendor/bin/contao-console contao:migrate``

Afterwards, the Drop step can be performed.

.. note:: MySQL on Windows treats table and column names as case-insensitive — therefore,
   changing the capitalisation of a name via MM is generally not possible.

.. |br| raw:: html

   <br />
