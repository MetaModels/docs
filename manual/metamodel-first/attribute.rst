.. _mm_first_attribute:

|img_fields_32| Attributes
===========================

After the table "mm_employeelist" was created in the database, the fields / table columns for
storing the data — i.e. the attributes — must now be created in it. This step is done via the
component of the same name "|img_fields| Attributes".

Based on the task requirements, the following fields are needed:

+------------------+----------------+----------+
| **Label**        | **Attr. name** | **Type** |
+------------------+----------------+----------+
| Last name        | name           | Text     |
+------------------+----------------+----------+
| First name       | firstname      | Text     |
+------------------+----------------+----------+
| E-mail           | email          | Text     |
+------------------+----------------+----------+
| Department       | department     | Text     |
+------------------+----------------+----------+
| Published        | published      | Checkbox |
+------------------+----------------+----------+

In the first step, switch to the "Attributes" component in the MetaModel "Employee List" by
clicking the icon |img_fields|. Then create the first attribute via
"|img_new| New attribute". Clicking "|img_new| New attribute" does not immediately open the
input mask, but instead shows a "|img_pasteafter| clipboard icon" — click on this (see
screenshot).

|img_attribute_01|

Clicking the "|img_pasteafter| clipboard icon" opens the input mask for the attribute. First,
select the attribute type "Text" from the selection list, and after the input mask refreshes,
the necessary fields are ready for input. These are filled in for the first attribute "Last name"
as shown in the screenshot.

|img_attribute_02|

"Save and close" creates the attribute "Last name", i.e. the column "name" is created in the
database table, and you are then returned to the attribute overview. These steps for creating an
attribute are now repeated for first name, email, and department.

For the "Published" attribute, a new attribute is also created, but with the attribute type
"Checkbox" selected. In the attribute's "Advanced settings", the "Publish" option is activated
(see screenshot).

|img_attribute_03|

The list of created attributes should now be shown as in the screenshot.

|img_attribute_04|


.. |img_fields_32| image:: /_img/icons/fields_32.png
.. |img_fields| image:: /_img/icons/fields.png
.. |img_new| image:: /_img/icons/new.gif
.. |img_pasteafter| image:: /_img/icons/pasteafter.gif

.. |img_attribute_01| image:: /_img/screenshots/metamodel_first/attribute_01.png
.. |img_attribute_02| image:: /_img/screenshots/metamodel_first/attribute_02.png
.. |img_attribute_03| image:: /_img/screenshots/metamodel_first/attribute_03.png
.. |img_attribute_04| image:: /_img/screenshots/metamodel_first/attribute_04.png

.. |br| raw:: html

   <br />

.. |nbsp| unicode:: 0xA0
   :trim:
