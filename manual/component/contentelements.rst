.. _component_contentelements:

Content Elements/Modules for Frontend Output
=============================================

.. note:: Create a MetaModel list as a content element or frontend module for display in the
  frontend; optionally, a filter can also be created as a content element or frontend module

Introduction
------------

A list and a filter element are available for frontend output. These can be used both as
content elements and as frontend modules in Contao. There is no difference in the configuration
options between content element and module.

Using modules is recommended when you want to output the same list/filter in multiple places
but only enter the settings once.

For the list display, the most important selection options include the selection of the
MetaModel (where does the data come from), the render setting and template selection (how
is the data displayed), and optionally the filter setting (which data is output).

Note that a detail view with one item is also just a "list display", but with appropriate
filtering for output.

For the filter settings, the most important selection options are the choice of MetaModel
(on what basis should filtering occur) and the choice of filter set (which filtering should
be used).

There is also a content element/module "Filter reset" for resetting all filter settings in
the frontend.

For certain alias-filter combinations, the route priority may need to be set in the page
settings — see :ref:`rst_cookbook_tips_set-route-priority`.

.. seealso:: For storing Contao content elements directly in a MetaModels record, the attributes
   :ref:`Content of an article <component_attribute_contentarticle>` (monolingual) and
   :ref:`Translated content of an article <component_attribute_translatedcontentarticle>`
   (multilingual) are available.


Options CE/Module List
----------------------

* **MetaModel settings**: |br|
  Selection of the MetaModel for the data source, as well as offset and limit of the list
* **MetaModel render setting**: |br|
  Selection of a created render setting, selection of the template
  ce/mod_metamodel_list and an option to output the data unparsed; |br|
  The template defines the wrapper surrounding the list, containing the list and pagination;
  if you want to influence the output of the items in the output list, this should be done in
  the template of the render setting (metamodel_prerendered); outputting data unparsed can be
  advantageous when a large amount of data is to be output, potentially saving unnecessary/
  duplicate template parsings.
* **MetaModel pagination**: |br|
  Pagination allows the output of the total list to be divided into individual pages.
  Overriding various default parameters is possible.
* **MetaModel filter**: |br|
  Selection of a created filter set; |br|
  If the "Static parameter" option is set in a "Simple lookup" filter rule, a select field
  for value selection appears here
* **Sorting**: |br|
  Here the sorting of list elements is set. |br|
  If manual sorting of items was done, choose "Sorting". Custom sorting, e.g. by multiple
  attributes, can be done via the "Custom SQL" filter rule
  (:ref:`see cookbook <rst_cookbook_filter_custom-sql_sortierung-der-ausgabe-nach-mehr-als-einem-attribut-fest>`).
  If the "Allow overriding the sorting" parameter is set, sorting can be overridden via URL,
  e.g. according to the schema ``/orderBy/<column name of attribute>/orderDir/<DESC || ASC>``
  or as a GET parameter. A method is available for creating sorting links for each attribute
  — more about this in the :ref:`"Cookbook" <rst_cookbook_templates_fe_list_sorting>`.
  Overriding various default parameters is possible.
* **Parameter settings**: |br|
  Parameter settings allow custom parameters to be easily passed to the template — more about
  this in the :ref:`"Cookbook" <rst_cookbook_templates_fe_list_parameters>`.


Options CE/Module Filter
------------------------

* **MetaModel**: |br|
  Selection of the MetaModel that forms the basis of the filtering
* **Filter settings to apply**: |br|
  Selection of a created filter set
* **Attributes**: |br|
  Filter rules of the filter setting that should be displayed in the frontend
* **Update on change**: |br|
  If this option is set, instead of a submit button, the filter form is sent directly
  after an input/selection.
* **Redirect page**: |br|
  The redirect page uses the filter parameters in the URL to navigate to the selected page.

.. note:: Redirect page settings from MM 2.3

From MM 2.3 it is possible to specify a form ID. This allows another filter to take over
the processing of the data — see :ref:`rst_cookbook_filter_filter-with-forwarding`.

The previous variant of including the filter as a module can still be used in MM 2.3.

.. note:: Redirect page settings up to MM 2.3

If you want to also include the same filter on the redirect page, this must be done via module.
You can place a filter and the list on different pages and define a redirect page on the filter
element. However, for the POST parameters of the filter element to become GET parameters for
the list, the same filter element must be built into the list page — it is sufficient if the
filter element is present as a hidden content element.

There is a security check by Contao that only identical forms may process the same data, i.e.
the filter element must be created as a module and built into both the page with the visible
filter and the list page.

The filter can be triggered via button or automatically via JavaScript when filter values are
changed in a filter widget (checkbox "Update on change").

.. note:: JavaScript from MM 2.2 no longer requires MooTools or jQuery ("Vanilla Script").

If you want to intervene in the JavaScript process, this is possible with various calls —
see the comments in the JavaScript file ``metamodels.js``.

Example for a custom call of 'submitonchange':

.. code-block:: js
   :linenos:

    <script>
    // Remove 'submitonchange'.
    window.MetaModelsFE.removeClassHook('submitonchange', window.MetaModelsFE.applySubmitOnChange);
    // Add own 'submitonchange'.
    window.MetaModelsFE.addClassHook('submitonchange', (el, helper) => {
        helper.bindEvent({
            object: el,
            type  : 'change',
            func  : (event) => {
                // Your code...
            },
        });
    });
    </script>

Example for a custom call of 'submitonchange' when multiple filter elements are on the page:

.. code-block:: js
   :linenos:

    <script>
    window.MetaModelsFE.addClassHook('submitonchange', (el, helper) => {
        // Check right element.
        if (el.withoutChange) {
             return;
        }
        // Remove 'submitonchange'
        helper.unbindEvents({object: el, type: 'change'});
        // Add own 'submitonchange'.
        helper.bindEvent({
            object: el,
            type  : 'change',
            func  : (event) => {
                // Own code...
            },
        });
    });
    </script>

Options CE/Module Filter Reset
-------------------------------

With this element, a button for resetting all filter entries (reset) can be inserted. As a
setting, there is the option to select a custom template and to define a URL fragment as an
anchor.


Procedure
---------

The content element or frontend module is created analogously to the classic Contao elements,
including the usual options such as activating access protection or specifying CSS IDs/classes.

.. seealso:: In the cookbook:

   * :ref:`rst_cookbook_specials_ce_element_for_editors`
   * :ref:`rst_cookbook_templates_fe_redirect_to_list`


.. |img_filter| image:: /_img/icons/filter.png

.. |br| raw:: html

   <br />
