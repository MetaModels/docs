.. _rst_cookbook_sql-tips:

SQL Tips
=========

Even though MetaModels takes care of a lot of programming work, it is sometimes
necessary to dive directly into the database layer — either to inspect data or
to modify it.

These tips assume familiarity with tools like phpMyAdmin and the basics of (My)SQL.

The following tips show how to view or modify data:

Displaying BLOB fields:
***********************

.. code-block:: sql
   :linenos:

   SELECT CONVERT(my_blob_attribute USING utf8) AS 'blob_as_text' FROM table

   > a:5:{s:9:"invoiceid";s:1:"8";s:8:"balance";i:5;s:14:"broughtforward";i:3;s:6:"userid";s:5:"13908";s:10:"customerid";s:1:"3";}


Extracting serialized data:
****************************

.. code-block:: sql
   :linenos:

   SELECT
   SUBSTRING_INDEX(SUBSTRING_INDEX(blob_as_text,';',1),':',-1) AS fieldname1,
   SUBSTRING_INDEX(SUBSTRING_INDEX(blob_as_text,';',2),':',-1) AS fieldvalue1,
   SUBSTRING_INDEX(SUBSTRING_INDEX(blob_as_text,';',3),':',-1) AS fieldname2,
   SUBSTRING_INDEX(SUBSTRING_INDEX(blob_as_text,';',4),':',-1) AS fieldvalue2,
   SUBSTRING_INDEX(SUBSTRING_INDEX(blob_as_text,';',5),':',-1) AS fieldname3,
   SUBSTRING_INDEX(SUBSTRING_INDEX(blob_as_text,';',6),':',-1) AS fieldvalue3,
   SUBSTRING_INDEX(SUBSTRING_INDEX(blob_as_text,';',7),':',-1) AS fieldname4,
   SUBSTRING_INDEX(SUBSTRING_INDEX(blob_as_text,';',8),':',-1) AS fieldvalue4,
   SUBSTRING_INDEX(SUBSTRING_INDEX(blob_as_text,';',9),':',-1) AS fieldname5,
   SUBSTRING_INDEX(SUBSTRING_INDEX(blob_as_text,';',10),':',-1) AS fieldvalue5
   FROM table;


The numbers 1 to 10 represent the count and position of semicolons in a serialized
string. Using the first example, ``blob_as_text,';',2`` is the second semicolon, i.e.
at ``..."8";s:8...``, and gives ``"8"`` as "fieldvalue1".

Trimming quotation marks:
**************************

.. code-block:: sql
   :linenos:

   SELECT TRIM(BOTH '"' FROM fieldvalue1) AS 'fieldvalue1_pure' FROM table

This command removes the leading and trailing quotation marks from the previous
example — the result would then be ``8``.

Restricting to a serialized value in WHERE:
*******************************************

For example, if the multi-select ([tags]) attribute has a relation to the user table
(tl_user) but you only want users from a specific user group, you need to filter on the
``groups`` column. In ``groups``, the group membership is stored as a serialized array,
so the serialized string must be searched.

In the multi-select attribute settings, a SQL filter can be applied as follows:

.. code-block:: sql
   :linenos:

   CONVERT(tl_users.groups USING utf8) LIKE '%"2"%'

This causes only users of user group ``2`` to be shown in the input mask.

Searching for files by UUID:
*****************************

The UUID of a file or folder can be read in the file manager via the info button.
Searching in the database is somewhat more difficult, as UUIDs are not stored in
plain text in the database. For the search, the UUID must be converted first. This
can be done by removing the hyphens and prepending "0x". For a UUID
"2abbf0c1-e76f-43e5-a123-00ac10d40e00" this would be e.g.:

.. code-block:: sql
   :linenos:

   SELECT * FROM tl_files
   WHERE uuid = 0x2abbf0c1e76f43e5a12300ac10d40e00

   -- or via automatic conversion
   SELECT * FROM tl_files
   WHERE LOWER(CONCAT(
        LEFT(HEX(uuid), 8),
        '-', MID(HEX(uuid), 9,4),
        '-', MID(HEX(uuid), 13,4),
        '-', MID(HEX(uuid), 17,4),
        '-', RIGHT(HEX(uuid), 12))
      ) = '2abbf0c1-e76f-43e5-a123-00ac10d40e00';
