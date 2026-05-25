.. _rst_cookbook_rendering_encrypt-email:

Rendering: Encrypting E-Mail Output
=====================================

.. note:: Encryption of e-mails for HTML5 output is now automatically included
   in the rendering of text attributes — see `Github <https://github.com/MetaModels/core/issues/1233>`_

E-mail addresses entered in Contao are output in encrypted form in the source
code to make automatic harvesting of e-mail addresses more difficult.

MetaModels does not have a dedicated "e-mail attribute" for this option — but
the feature can be quickly added with a customised template for a "Text"
attribute.

All that is needed is to create and activate a special template for the
rendering. Follow these steps:

* In the backend under "Templates", create a template "mm_attr_text.html5"
* Rename the template to "mm_attr_text_email.html5"
* In the new template, add the source code |br|
  ``<span class="text<?= $this->additional_class ?>">{{email::<?= $this->raw ?>}}</span>``
* In the render settings of the corresponding text attribute, select the new
  template "mm_attr_text_email"

|img_encrypt-email|


.. |br| raw:: html

   <br />

.. |img_encrypt-email| image:: /_img/screenshots/cookbook/renderings/encrypt-email.jpg
