.. _rst_cookbook_templates_flatpickr-integration:

Simple Date Picker for the From-To Filter Rule via Flatpickr Integration
=========================================================================

If you want a date picker in the FE widget of the From-To filter rule ("value from/to for a
date field"), this can be achieved with the following adjustments:

In the backend, create the template ``mm_filteritem_default.html5``, rename it to
``mm_filteritem_flatpickr.html5``, and select it in the filter settings.

The following lines need to be added to the template:

At the top, include the Flatpickr files — these can be found at
`Flatpickr <https://flatpickr.js.org>`_:

.. code-block:: php
   :linenos:

   <?php
   $GLOBALS['TL_JAVASCRIPT'][] = 'files/resources/flatpickr/flatpickr.min.js';
   $GLOBALS['TL_JAVASCRIPT'][] = 'files/resources/flatpickr/l10n/en.js';
   $GLOBALS['TL_JAVASCRIPT'][] = 'files/resources/flatpickr/plugins/rangePlugin.js';
   $GLOBALS['TL_CSS'][]        = 'files/resources/flatpickr/flatpickr.min.css';
   ?>

At the end, add the following JavaScript code — here the column name of the attribute is
``startDate`` and the RangePlugin is used. Further settings can be found in the
`Flatpickr documentation <https://flatpickr.js.org>`_:

.. code-block:: php
   :linenos:

   <script>
   flatpickr('#ctrl_startDate_0', {
      locale: "en",
      minDate: "today",
      enableTime: false,
      allowInput: true,
      disableMobile: true,
      dateFormat: "Y-m-d",
      defaultDate: ["today", new Date().fp_incr(14)],
      "plugins": [new rangePlugin({ input: "#ctrl_startDate_1"})]
   });
   </script>
