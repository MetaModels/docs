.. _component_attribute_rating:

|svg_attr_rating_22| |img_star_full| Rating
===========================================

The "Rating" attribute provides a star rating system. Visitors can rate items via an AJAX
widget in the frontend. The backend displays the number of ratings and the average value.
Typical use cases:

* Product ratings in shops
* Rating of articles, recipes, or events
* User surveys with star scale

The actual rating is done exclusively via AJAX from the frontend —
the field is read-only in the backend. A session-based lock is applied per visitor and item
to prevent multiple ratings.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_rating


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings, the attribute offers the following specific options:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Maximum value
     - Defines how many stars can be awarded at most (e.g. ``5`` for a five-star rating).
   * - Half ratings
     - Enables 0.5 steps instead of whole numbers, so that e.g. 3.5 out of 5 stars is possible.
   * - Image for "empty star"
     - Custom image displayed as an unfilled star. If no image is selected, the default
       icon from the extension is used.
   * - Image for "full star"
     - Custom image displayed as a filled star.
   * - Image for hover effect
     - Custom image displayed when hovering with the mouse.


Settings in Render Settings
-----------------------------

The attribute has its own render setting:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Disable voting
     - Disables the ability to vote in the frontend for this render setting. The result
       is then only displayed without the ability to submit a new rating.
   * - Template
     - Selection of a custom template for the output of the rating display.
   * - CSS class
     - Optional CSS class added to the output element.


Settings in the Input Form
----------------------------

The attribute is read-only in the backend — ratings can only be submitted via the frontend
AJAX widget. When added to an input form, the current rating average and number of votes
are displayed.

**Display**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Backend class
     - CSS classes for the display in the backend form.
   * - Template for backend
     - Selection of a custom widget template for the backend form.


Filter Rules
------------

The rating attribute does not support its own filter rules — rating data cannot be directly
used as filter criteria in the frontend. However, sorting by rating (average value, then
number of votes) is possible.


Special Functions
-----------------

**Storage**

The rating data is **not** stored in the MetaModel table, but in the dedicated table
``tl_metamodel_rating`` with the columns:

* ``mid`` — MetaModel ID
* ``aid`` — Attribute ID
* ``iid`` — Item ID
* ``votecount`` — number of votes submitted
* ``meanvalue`` — calculated average value as a percentage (``double``)

**Voting logic**

Each submitted vote is processed via AJAX. MetaModels checks based on the visitor session
whether a vote has already been submitted for this item. The average value is calculated
using the following formula:

``(1 / (rating_max × votecount)) × (previous total + new vote)``

The value is stored as a percentage (0–1).

**Default icons**

If no custom image is selected for star symbols, the extension uses default images from
the bundle: ``star-empty.png``, ``star-full.png``, ``star-hover.png``.

**Sorting**

Items are sorted by average value in descending order; in the event of a tie, the number
of votes decides. Items without a rating are placed at the end of the list.


.. |svg_attr_rating_22| image:: /_img/icons_svg/star.svg
   :width: 22px
.. |img_star_full| image:: /_img/icons/star-full.png
.. |br| raw:: html

   <br />
