.. _rst_cookbook_frontend_output-item-count:

Displaying the Number of Items
===============================

To display the number of items in a frontend template, two variables are
available:

* `count($this->data)` — returns the current number of items being output in
  the template, i.e. only the items currently being rendered are counted; if
  pagination is set, at most the pagination size is output
* `$this->total` — returns the total number of items output in the template;
  pagination has no effect on this value

The respective template can be extended with the following output, for example:

.. code-block:: php
   :linenos:

    <?php if (count($this->data)): ?>

    <div class="layout_full">
        <div class="count_data">Count data: <?= count($this->data) ?></div>
        <div class="count_total">Count total: <?= $this->total ?></div>
        <?php foreach ($this->data as $arrItem): ?>
        <div class="item <?= $arrItem['class'] ?>">
    //....


