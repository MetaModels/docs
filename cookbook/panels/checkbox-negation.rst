.. _rst_cookbook_panels_checkbox-negation:

View condition: Display s.th., if the checkbox is not activated
===============================================================

If you want to create a view condition, which enables you to dispay a field, if a checkbox is **not** checked, this will be not possible with a trigger on the "inactive" value of the checkbox.

This is due to the fact that MetaModels treats the value "unchecked" differently from the Contao core - the Contao core will store nothing '' for "unchecked" instead of a null(0). This can not be processed by MetaModels or the DCG at the moment.

This problem can be fixed with a workaround: The visibility is triggered by "checked", but the test is inverted with NOT. To achieve that, a condition NOT has to be created in the view conditions and inside this condition the test whether the checkbox is "active" (see screenshot).

|img_checkbox-negation_01|

The following two screenshots show the hiding of an email input mask with the checkbox set.

Email shown

|img_checkbox-negation_02|

Email hidden

|img_checkbox-negation_03|

.. |img_checkbox-negation_01| image:: /_img/screenshots/cookbook/panels/checkbox-negation_01.jpg
.. |img_checkbox-negation_02| image:: /_img/screenshots/cookbook/panels/checkbox-negation_02.jpg
.. |img_checkbox-negation_03| image:: /_img/screenshots/cookbook/panels/checkbox-negation_03.jpg


.. |br| raw:: html

   <br />
