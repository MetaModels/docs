.. _mm_first_dca:

|img_dca_32| Input screens
==========================

In this step we will create the input screen for the MetaModel "Employee list", which will enable us to store attribute data in our database.

First go to the MetaModels overview in the back end to see your MetaModel "Employee list". Next, click onto  the icon "|img_dca| Define input screens". The view will change to the overview of input screens which is actually empty.

Click "|img_new| New input screen" and the input mask for the input screen settings will open. For the input field "Name" you might want to enter a name such as "Input". Another important setting is "Integration": Here you should select "Standalone" for our example. Then another drop-down menu named "Backend section" appears beneath. Here you select "MetaModels".
Additionally you should activate all the three checkboxes under "Data manipulation permissions" -  see screenshot. Then hit "Save and close" to save your setting.

|img_dca_01_en|

Now you should be able to see your first entry "Input" in the input screens overview - see screenshot.

|img_dca_02_en|

Click onto the icon "|img_dca_setting| Settings" in order to open the next screen to add some attributes. Hee you can select and activate the attributes which you want to be shown in your input screen.

Just like in the render settings you can also here add all attributes with one step. In the header click on the icon "|img_dca_add| Add all", then hit the button "Continue" and then "Save and close". Now you have added all available attributes to your input screen. Note that the attributes are not activated by default -  you can do this easily by clicking on the "eye icon".

For this example we will activate all the attributes - the list should now look like in the screenshot below.

|img_dca_03_en|

Note that the input mask is not visible in the back end yet. You'll be able to see it when the next step :ref:`component_dca-combine` is finished.


.. |img_dca_32| image:: /_img/icons/dca_32.png
.. |img_dca| image:: /_img/icons/dca.png
.. |img_dca_setting| image:: /_img/icons/dca_setting.png
.. |img_dca_setting_add| image:: /_img/icons/dca_setting_add.png
.. |img_dca_add| image:: /_img/icons/dca_add.png
.. |img_dca_groupsortsettings| image:: /_img/icons/dca_groupsortsettings.png
.. |img_dca_condition| image:: /_img/icons/dca_condition.png
.. |img_new| image:: /_img/icons/new.gif
.. |img_edit| image:: /_img/icons/edit.gif

.. |img_dca_01_en| image:: /_img/screenshots/metamodel_first/dca_01_en.png
.. |img_dca_02_en| image:: /_img/screenshots/metamodel_first/dca_02_en.png
.. |img_dca_03_en| image:: /_img/screenshots/metamodel_first/dca_03_en.png
