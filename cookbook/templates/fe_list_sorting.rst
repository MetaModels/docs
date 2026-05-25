.. _rst_cookbook_templates_fe_list_sorting:

Links for Toggling the Sort Order of a MM List
===============================================

.. note:: This feature is available from MM 2.2.

In the list output settings (CE/FE module) there is an option to allow the default sort order to
be overridden ("Allow overriding the sort order"). When this option is enabled, various parameters
can be set:

* Slug/GET key for overriding ``orderBy`` as the key for the attribute to sort by
* Slug/GET key for overriding ``orderDir`` as the key for the sort direction
* URL fragment if the link should jump to a specific anchor on the page

|img_sorting_options|

The desired links for custom sorting can be added in the MM list template or elsewhere.

.. note:: This feature is available from MM 2.3.

To simplify usage in the MM list template, it is possible to generate various sorting links for
each attribute. There is a "toggle link" that switches to the other sort direction, as well as
individual links for ascending and descending — corresponding CSS classes and an active parameter
are also passed along. The complete link including CSS classes can also be generated directly.

The following snippet shows example calls for the "Name" attribute with column name ``name``:

.. code-block:: php
   :linenos:

   <?php
   // Variant 1 with 'generateSortingLink':
   <?php if ($sortingLinkToggle = $this->generateSortingLink('name', 'toggle')): ?>
   <a href="<?= $sortingLinkToggle['href'] ?>" class="<?= $sortingLinkToggle['class'] ?>" data-escargot-ignore rel="nofollow"><?= $sortingLinkToggle['label'] ?> (toggle)</a><br>
   <?php endif; ?>
   <?php if ($sortingLinkAsc = $this->generateSortingLink('name', 'asc')): ?>
   <a href="<?= $sortingLinkAsc['href'] ?>" class="<?= $sortingLinkToggle['class'] ?>" data-escargot-ignore rel="nofollow"><?= $sortingLinkAsc['label'] ?> (asc)</a><br>
   <?php endif; ?>
   <?php if ($sortingLinkDesc = $this->generateSortingLink('name', 'desc')): ?>
   <a href="<?= $sortingLinkDesc['href'] ?>" class="<?= $sortingLinkToggle['class'] ?>" data-escargot-ignore rel="nofollow"><?= $sortingLinkDesc['label'] ?> (desc)</a><br>
   <?php endif; ?>

   // Variant 2 with 'renderSortingLink':
   <?= $this->renderSortingLink('name', 'toggle') ?> (toggle)<br>
   <?= $this->renderSortingLink('name', 'asc') ?> (asc)<br>
   <?= $this->renderSortingLink('name', 'desc') ?> (desc)<br>

   // List...
   <?php foreach ($this->data as $arrItem): ?>

Note that when linking to the default sort order settings, the slug/GET parameters are removed —
only the URL fragment is retained. The ``data-escargot-ignore`` attribute prevents the link from
being picked up by the Contao crawler for search indexing.

Calling ``generateSortingLink`` with the parameters "column name" of the attribute and sort type
returns the following values:

* "attribute": reference to the attribute
* "name": name of the attribute
* "href": link for sorting
* "direction": current sort direction (``asc`` || ``desc``)
* "active": ``true`` if this is the currently sorted attribute, otherwise ``false``
* "class": CSS classes
* "label": label

``renderSortingLink`` generates a complete link — the text can be customised by adjusting the
language file.

.. |img_sorting_options| image:: /_img/screenshots/cookbook/templates/sorting_options.jpg


