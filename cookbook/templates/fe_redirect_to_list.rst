.. _rst_cookbook_templates_fe_redirect_to_list:

Automatic Redirect from Detail Page to List Page or "Error 404"
================================================================

The data output on the detail page is often controlled or filtered via one or more parameters —
usually via the ``auto_item``.

If no record can be found due to the filter, or if the detail page is accessed without providing
the (filter) parameter in the URL, a message such as "No data could be found" is displayed.

If this is not desired and the user should instead be redirected directly to the list view, this
can be achieved with the following code in the detail view template:

.. code-block:: php
   :linenos:

    // redirect if data empty
    if (!count($this->data)) {
        $pageId  = 192; // Page id
        $page    = \PageModel::findByPK($pageId);
        $pageURL = $page->getFrontendUrl();
        \Controller::redirect($pageURL);
    }

If the Contao base page is accessed without providing the (filter) parameter, an "Error 404" can
also be delivered automatically. To do this, enable the "Requires item" checkbox in the page
settings.
