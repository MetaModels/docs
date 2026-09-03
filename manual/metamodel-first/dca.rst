.. _mm_first_dca:

|svg_dca_32| |img_dca_32| Input Masks
=======================================

In this step, the input mask for the MetaModel "Employee List" is created, through which the
attribute data is stored in the database.

To access the input masks, activate the MetaModels overview again so that the "Employee List"
entry is visible. Then click the icon "|svg_dca_22| |img_dca| Input masks" and the view switches to the input
masks overview — which is currently still empty.

After clicking "|img_new| New input mask", the mask for the input mask settings opens immediately.
In the "Name" input field, enter a name such as "Input". Another important setting is the
"Integration" selection, where "Independent" should be selected, and in the "Backend section"
dropdown that appears, "MetaModels" should be selected. Additionally, all three checkboxes of the
"Data manipulation permissions" block should be activated — see screenshot. Save the entry with
"Save and close".

|img_dca_01|

The input masks overview should now show the first entry "Input" — see screenshot.

|img_dca_02|

Clicking the icon "|svg_dca_setting_22| |img_dca_setting| Settings" opens the next level for the attributes. Here,
the attributes to be displayed in the input mask are selected or activated.

As with the render settings, the created attributes can be added in one step here too. To do
this, click the header icon "|svg_dca_add_22| |img_dca_add| Add all" and then confirm the "Continue" and "Save
and close" buttons. All existing attributes are now added to the input mask. By default, the
attributes are not activated — this can easily be done via the "eye icon".

In this example, all attributes are activated — the attribute list should now look like the
screenshot.

|img_dca_03|

The input mask is still not visible in the backend. This only happens once the
:ref:`component_dca-combine` step is completed.


.. |svg_dca_32| image:: /_img/icons_svg/dca.svg
   :width: 32px
.. |img_dca_32| image:: /_img/icons/dca_32.png
.. |svg_dca_22| image:: /_img/icons_svg/dca.svg
   :width: 22px
.. |img_dca| image:: /_img/icons/dca.png
.. |svg_dca_setting_22| image:: /_img/icons_svg/dca_setting.svg
   :width: 22px
.. |img_dca_setting| image:: /_img/icons/dca_setting.png
.. |img_dca_setting_add| image:: /_img/icons/dca_setting_add.png
.. |svg_dca_add_22| image:: /_img/icons_svg/dca_add.svg
   :width: 22px
.. |img_dca_add| image:: /_img/icons/dca_add.png
.. |img_dca_groupsortsettings| image:: /_img/icons/dca_groupsortsettings.png
.. |img_dca_condition| image:: /_img/icons/dca_condition.png
.. |img_new| image:: /_img/icons/new.gif
.. |img_edit| image:: /_img/icons/edit.gif

.. |img_dca_01| image:: /_img/screenshots/metamodel_first/dca_01.png
.. |img_dca_02| image:: /_img/screenshots/metamodel_first/dca_02.png
.. |img_dca_03| image:: /_img/screenshots/metamodel_first/dca_03.png
