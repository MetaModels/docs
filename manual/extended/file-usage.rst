.. _rst_extended_file-usage:

File-Usage Integration
######################

.. note:: Available from MetaModels 2.3 — to activate please send an email to mail@metamodel.me.

The `File-Usage <https://github.com/inspiredminds/contao-file-usage>`_ extension allows you to
see in the file manager whether and where a file is used in Contao. Support for MM is available
from File-Usage version 4.1.

Corresponding providers have been created to display the images embedded in MetaModels records.
The following attributes are currently supported:

* :ref:`Content article <component_attribute_contentarticle>`
* :ref:`File <component_attribute_file>`
* :ref:`Long text <component_attribute_longtext>`
* :ref:`Multi-table (MCW) <component_attribute_tablemulti>`
* :ref:`Translated content article <component_attribute_translatedcontentarticle>`
* :ref:`Translated file <component_attribute_translatedfile>`
* :ref:`Translated long text <component_attribute_translatedlongtext>`
* :ref:`Translated multi-table (MCW) <component_attribute_translatedtablemulti>`

Depending on the attribute, the stored UUID(s) of the file or files are searched, or existing
insert tags with file references (`file`, `picture`, `figure`) are searched.

As of `File-Usage version 4.1.0 <https://github.com/inspiredminds/contao-file-usage/releases/tag/4.1.0>`_,
textual path references such as `/file/content/my_file.jpg` in the HTML attributes `href` and
`src` are also searched — for example in text fields.

For MetaModels, dedicated outputs are provided showing the model name, the attribute name, and
for multilingual attributes also the language. Clicking the pencil icon leads directly to the
corresponding record — see screenshot. For multilingual MetaModels, the input mask language is
set accordingly via a GET parameter.

|img_mm_file-usage|


Donations
---------

Thanks for the donations* for the extension to (target amount 2,613.75 €):

* `AntwortInternet <https://www.antwortinternet.com/>`_: 340 €
* `AntwortInternet <https://www.antwortinternet.com/>`_: 340 €
* `P KREATIV <https://p-kreativ.at/>`_: 250 €
* `GUTcert <https://www.gut-cert.de/>`_: 340 €

(*Donations are net amounts)


.. |manual@metamodel.me| raw:: html

   <a href="mailto:manual@metamodel.me">on request</a>

.. |img_mm_file-usage| image:: /_img/screenshots/extended/file-usage/mm_file-usage.png


