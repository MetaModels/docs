.. _mm_first_contentelements:

Content Elements/Modules for Frontend Output
=============================================

Once all components for data entry are configured, the data output can be set up. Various
options are available for data output — in this example, the output will use the article content
element "MetaModel list".

As a preparation for the output, a corresponding page must exist in Contao with an article that
contains the content element. A new content element is created with the following settings
activated (see screenshot):

* Element type: MetaModel list
* Sort by: Name
* Filter setting to apply: Published
* Render setting to apply: FE List

|img_contentelements_01|

After "Save and close", the content element is available and the display can be checked in the
frontend.

The display should now show the message "Your search returned no results." since no data has
been entered yet.

To test the display, it is necessary to create some records in the employee list. To do this,
click the icon "|img_metamodels| Employee List" under "MetaModels" in the left backend
navigation, and then click the icon "|img_new| New record".

The input mask opens with the predefined fields (attributes), which can be filled with initial
data (see screenshot).

|img_contentelements_02|

After "Save and close", the record is visible with the activated attributes from the render
setting "BE List" (last name and first name) (see screenshot).

|img_contentelements_03|

The record can be edited again via the pencil icon, and the "Published" status can be toggled
via the "eye" (as an alternative to the checkbox in the input mask).

The frontend output should now look approximately as follows (screenshot).

|img_contentelements_04|

If some test data is loaded into the database — or entered manually — the employee list in the
backend looks approximately as shown in the screenshot:

|img_contentelements_05|

And in the frontend as follows:

|img_contentelements_06|

For the frontend output, the attributes are rendered via the default template into individual
HTML div containers including specific CSS classes. Formatting can be done either via CSS or by
customizing the template so that the output is rendered as an HTML table.

With some CSS rules like the following:

.. code-block:: css
   :linenos:

	.ce_metamodel_content .item {
	    display: table;
	    width: 100%;
	}
	.ce_metamodel_content .item.even {
	    background-color: #f4f2f0;
	    border-bottom: 1px solid #d4cbc5;
	    border-collapse: collapse;
	}
	.ce_metamodel_content .item.odd {
	    background-color: #f6f6f6;
	    border-bottom: 1px solid #d4cbc5;
	    border-collapse: collapse;
	}
	.ce_metamodel_content .item .field {
	    display: table-cell;
	}
	.ce_metamodel_content .item .field.name {
	    width: 20%;
	}
	.ce_metamodel_content .item .field.firstname {
	    width: 20%;
	}
	.ce_metamodel_content .item .field.email {
	    width: 40%;
	}
	.ce_metamodel_content .item .field.department {
	    width: 20%;
	}

the output already looks better — see screenshot:

|img_contentelements_07|


.. _mm_first_contentelements_detailpage:
Detail Page of a Record
------------------------

Typically, the list view does not display all content of a record, but only those fields needed
for searching or selection. The complete record can be presented on a detail page.

For this, a detail page must first be created in Contao — e.g. ``domain.tld/employee-details.html``.

A content element MM list is inserted into this page as a module or CE — MetaModel is again
``mm_employees``.

A render setting for the output is also needed — for the detail page you should create a separate
render setting "FE - Details". With a custom :ref:`template <component_templates>`, the output
can be individually designed.

To ensure only the desired record — and not all records — is output on the page, a corresponding
filter is needed. The :ref:`"Simple lookup" <component_filter_simplelookup>` filter rule is
recommended, with an :ref:`alias <component_attribute_alias>` or
:ref:`translated alias <component_attribute_translatedalias>` as the attribute. Additional
filter rules such as "Published" etc. are also possible as needed. The created filter must be
selected in the MM list for the detail page.

With a URL like ``domain.tld/employee-details/alias/avery-amir.html``, the corresponding
record should be output.

To automatically generate links to the detail page in the list view, in the render setting of
the list in the "Jump-to settings" section, the detail page must be selected along with the
corresponding filter. The output of the links in the template is in the ``actions`` node.




.. |img_new| image:: /_img/icons/new.gif
.. |img_metamodels| image:: /_img/icons/metamodels.png

.. |img_contentelements_01| image:: /_img/screenshots/metamodel_first/contentelements_01.png
.. |img_contentelements_02| image:: /_img/screenshots/metamodel_first/contentelements_02.png
.. |img_contentelements_03| image:: /_img/screenshots/metamodel_first/contentelements_03.png
.. |img_contentelements_04| image:: /_img/screenshots/metamodel_first/contentelements_04.png
.. |img_contentelements_05| image:: /_img/screenshots/metamodel_first/contentelements_05.png
.. |img_contentelements_06| image:: /_img/screenshots/metamodel_first/contentelements_06.png
.. |img_contentelements_07| image:: /_img/screenshots/metamodel_first/contentelements_07.png
