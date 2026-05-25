.. _rst_cookbook_frontend_array-helper:

Array Helper
============

In many cases, a template for frontend output is built from HTML and "echo" of
the PHP variables from the output array.

With a debug output as described in :ref:`rst_cookbook_debug_templates`, you can
see the array available in the template with its nodes as a tree structure.

Transferring this into the template can become quite tedious, especially when
there are many relations to other MetaModels.

With the following "Array Helper", the array nodes are output in a way that
allows easy copy & paste into the template.

The respective template is extended with the following lines at the top:

.. code-block:: php
   :linenos:

    <?php
    // http://stackoverflow.com/a/14518402
    function printArray($array, $path=false, $top=true) {
        $data = ""; $delimiter = "~~|~~"; $p = null;
        if(is_array($array)){
            foreach($array as $key => $a){
                if(!is_array($a) || empty($a)){
                    if(is_array($a)){
                        $data .= $path."['{$key}'] = array();".$delimiter;
                    } else {
                        $data .= $path."['{$key}'] = \"".addslashes($a)."\";".$delimiter;
                    }
                } else {
                    $data .= printArray($a, $path."['{$key}']", false);
                }
            }
        }
        if($top){
            $return = "";
            foreach(explode($delimiter, $data) as $value){
                if(!empty($value)){ $return .= '$arrItem'.$value."\n"; }
            };

            return $return;
        }

        return $data;
    }

    echo "<!-- DEBUG START\n";
    echo "<pre>\n";
    // 0th node only
    //print_r($this->items->parseAll($this->getFormat(), $this->view)[0]);
    echo printArray($this->items->parseAll($this->getFormat(), $this->view)[0]);
    echo "</pre>\n";
    echo "DEBUG END -->\n";
    ?>

The template should then begin with the following lines — for a better view,
switch to source code view (Ctrl + U | Cmd + Alt + U):


.. code-block:: html
   :linenos:

   <html>
    <!-- DEBUG START
    <pre>
    $arrItem['raw']['id'] = "93";
    $arrItem['raw']['pid'] = "0";
    $arrItem['raw']['sorting'] = "0";
    $arrItem['raw']['tstamp'] = "1484897086";
    $arrItem['raw']['name'] = "0";
    $arrItem['raw']['firstname'] = "Amir";
    $arrItem['raw']['email'] = "Amir.Avery@mmtest.com";
    $arrItem['raw']['department']['__SELECT_RAW__']['id'] = "4";
    $arrItem['raw']['department']['__SELECT_RAW__']['pid'] = "0";
    $arrItem['raw']['department']['__SELECT_RAW__']['sorting'] = "0";
    $arrItem['raw']['department']['__SELECT_RAW__']['tstamp'] = "1442499032";
    $arrItem['raw']['department']['__SELECT_RAW__']['name'] = "Marketing";
    $arrItem['raw']['department']['__SELECT_RAW__']['alias'] = "marketing";
    $arrItem['raw']['department']['name'] = "Marketing";
    $arrItem['raw']['department']['alias'] = "marketing";
    $arrItem['text']['name'] = "Avery";
    $arrItem['text']['firstname'] = "Amir";
    $arrItem['text']['email'] = "Amir.Avery@mmtest.com";
    $arrItem['text']['department'] = "Marketing";
    $arrItem['attributes']['name'] = "Last name";
    $arrItem['attributes']['firstname'] = "First name";
    $arrItem['attributes']['email'] = "E-Mail";
    $arrItem['attributes']['department'] = "Department";
    $arrItem['html5']['name'] = "<span class=\"text\">0</span>";
    $arrItem['html5']['firstname'] = "<span class=\"text\">Amir</span>";
    $arrItem['html5']['email'] = "<span class=\"text\">Amir.Avery@mmtest.com</span>";
    $arrItem['html5']['department'] = "Marketing";
    $arrItem['class'] = "first even";
    $arrItem['jumpTo'] = array();
    </pre>
    DEBUG END -->
    ...
   </html>

In the template, the output of the department could then look like this:

.. code-block:: html
   :linenos:

   <html>
   ...
   <p><span class="label"><?= $arrItem['attributes']['department'] ?>:</span> <?= $arrItem['raw']['department']['name'] ?></p>
   ...
   </html>

The output can be removed by commenting out the output block, deleting it, or
switching to a different template.


