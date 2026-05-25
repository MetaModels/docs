.. _rst_cookbook_tips_speedup_backend:

Speeding Up the Backend View with Large Numbers of Records
==========================================================

With very large numbers of records — from around 5,000 or more — building the list view or
input mask can take a long time and be very memory-intensive. This depends on various factors
such as the attribute types in use or the panel settings of the list or input mask. The
following tips can help speed up the view:

1. Set a limit |br|
   In the input mask settings, add the key "limit" to the "Panel layout" field.
   This paginates the list view.
2. Remove or adjust filters |br|
   If individual attributes are enabled for filtering, these should not be used with very large
   datasets. Filters do not operate independently of each other, meaning that the queries to be
   executed can take a long time with increasing numbers of filters and records. As an
   alternative to filtering, attributes can be enabled for search instead. The result is the
   same narrowing of the list — just without predefined data in the panel.
   A further optimisation is planned for MM 3.x.
3. Add a database index to the table |br|
   Display performance with large datasets can be improved by creating one or more indexes.
   With small datasets, this can actually slow down query execution, which is why MM does not
   create these automatically. Columns used in a search should be given an index — for example
   the column "surname" in the table "mm_employees". This can be created with the following
   query: |br|
   ```create index mm_employees_surname_id_index on mm_employees (surname, id);``` |br|
   Delays in building input masks can also occur — for example when referencing other tables
   such as a select on members. In that case, also add an index to the corresponding column: |br|
   ```create index mm_employees_member_index on mm_employees (member);```

.. |br| raw:: html

   <br />
