.. _rst_cookbook_specials_export-excel:

Importing Data into a Spreadsheet
===================================

For analysis purposes such as graphical processing with charts or various
calculations, there is sometimes a request to export data to a spreadsheet
application such as MS Excel, OpenOffice Calc, or Google Sheets.

One option is to create an export of the current data in a corresponding format
(XLSX, ODS, XLS) (see `MM conference talk 2023 <https://www.e-spin.de/contao-metamodels/metamodels-vortrag-contao-konferenz-2023.html>`_).

Another simple approach is to dynamically fetch the data. For this, it is only
necessary to output the data as a table and thus make it ready for import. This
can be done with a dedicated frontend output page or by calling a `custom route <https://docs.contao.org/dev/framework/routing/#implementing-custom-routes>`_.

The corresponding applications can import this table with the data — not just
once, but depending on the type also when opening the file or continuously after
a specified time interval.

To prepare for the data import, the data must be output as a table. A dedicated
page can be set up for this, omitting superfluous elements such as header, footer,
etc. The data is output as a table via an appropriate template — e.g.

.. code-block:: php
   :linenos:

    <?php
    // templates/metamodel_pre_movies_table.html5
    if (count($this->data)): ?>
        <div class="layout_full">
            <table id="export">
                <thead>
                <tr>
                    <?php foreach ($this->data[0]['attributes'] as $attributeName): ?>
                        <th><?= $attributeName ?></th>
                    <?php endforeach; ?>
                </tr>
                </thead>
                <tbody>
                <?php foreach ($this->data as $arrItem): ?>
                    <tr>
                        <?php foreach ($arrItem['attributes'] as $field => $strName): ?>
                            <td><?= $arrItem['text'][$field] ?></td>
                        <?php endforeach; ?>
                    </tr>
                <?php endforeach; ?>
                </tbody>
            </table>
        </div>
    <?php else : ?>
        <?php $this->block('noItem'); ?>
        <p class="info"><?= $this->noItemsMsg ?></p>
        <?php $this->endblock(); ?>
    <?php endif; ?>

No pagination should be configured in the CE/FE module MM List settings. For
large numbers of records, the table output time can be improved by enabling the
checkbox "Do not output parsed items via '$data'" or by adding an index to the MM
table — more on this at :ref:`rst_cookbook_tips_speedup_backend`.


Data in Excel
-------------

The :download:`example file </_download/Movie-Database.xlsx.zip>` can be used
for import into Excel, or you can start with a new file. More details at
`Excel <https://support.microsoft.com/en-us/office/import-data-from-the-web-a1a6b325-17f3-45c8-ae72-c421cb2a8e90>`_.

In the "Data" tab, select the web as the data source.

|img_excel-export_01|

In the next step, enter the URL — in the example https://a-movie-database.metamodel.me/de/excel-connect.html.

|img_excel-export_02|

After selecting "Anonymous" as the connection type and clicking "Connect", a
wizard appears where you can select the appropriate table.

|img_excel-export_03|

Clicking "Load" completes the settings and the data is visible.

|img_excel-export_04|


Data in LibreOffice Calc
------------------------

See the `LibreOffice documentation <https://help.libreoffice.org/latest/en-US/text/scalc/01/04090000.html>`_
— after entering the URL, press Enter and wait a few seconds until the HTML
tables selection is populated.


Data in OpenOffice Calc
-----------------------

The :download:`example file </_download/Movie-Database.ods.zip>` can be used
for import into Calc, or you can start with a new spreadsheet.

Under "Insert", create a "Link to External Data".

|img_oo-export_01|

In the next step, enter the URL — if no entries appear in the "Available
tables/ranges" field after entering the URL, click the "..." button and paste
the URL into "File name", then click "Open". Then select the table "HTML_export"
(table ID "export") and click "OK".

|img_oo-export_02|

The data is then available in the spreadsheet.

|img_oo-export_03|


Data in Google Sheets
---------------------

The import into Google Sheets is done via a formula — enter the following formula
in cell A1:

``=importhtml("https://a-movie-database.metamodel.me/de/excel-connect.html", "table", 1)``

The first parameter is the URL, the second is the type, and the third is the
table number (starting with 1). After entering the formula, the data is loaded.

|img_google-sheet_01|


.. |img_excel-export_01| image:: /_img/screenshots/cookbook/specials/excel-export_01.jpg
.. |img_excel-export_02| image:: /_img/screenshots/cookbook/specials/excel-export_02.jpg
.. |img_excel-export_03| image:: /_img/screenshots/cookbook/specials/excel-export_03.jpg
.. |img_excel-export_04| image:: /_img/screenshots/cookbook/specials/excel-export_04.jpg
.. |img_oo-export_01| image:: /_img/screenshots/cookbook/specials/oo-export_01.jpg
.. |img_oo-export_02| image:: /_img/screenshots/cookbook/specials/oo-export_02.jpg
.. |img_oo-export_03| image:: /_img/screenshots/cookbook/specials/oo-export_03.jpg
.. |img_google-sheet_01| image:: /_img/screenshots/cookbook/specials/google-sheet_01.jpg
