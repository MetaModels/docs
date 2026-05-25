.. _component_dca-combine:

|img_dca_combine_32| Input/Render Assignments
==============================================

.. note:: Define access options for render settings and input forms;
  backend input access should be enabled at minimum for the 'Administrator' user group

Introduction
------------

Input/render assignments set the permissions for the created render settings.
The following select fields are available for each entry:

* Member group
* User group
* Render setting
* Input form

As a standard, an input form and a render setting should be enabled for the "Administrator"
user group for display and access in the backend.

It is possible to create multiple assignments and thereby control access to list output and
input forms. Input forms for members are only relevant for frontend editing.

When multiple assignments (rows) are created, they are processed "from top to bottom", i.e.
for the member or user group, the first specified group is evaluated as valid. Note that the
entry "*" represents a "catch all" and represents the settings for all remaining groups.

If you want, for example, that no "catch all" is applied in a row, or no group is matched,
you can create a member or user group, e.g. "empty", to which no member or user is assigned.


Procedure
---------

Make the selections in the predefined columns of the input/render assignments and save. The
MetaModel input options should now be visible in the backend.


.. |img_dca_combine_32| image:: /_img/icons/dca_combine_32.png
.. |img_dca_combine| image:: /_img/icons/dca_combine.png
