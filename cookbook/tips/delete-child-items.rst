.. _rst_cookbook_tips_delete_child_items:

Automatic Deletion of Records in Child Tables
=============================================

When tables are linked as child tables in MetaModels, child records are currently not
automatically deleted when the corresponding parent record is deleted. While DC_General supports
a "deep delete" function, this is not yet configurable for custom MM tables.

However, it can be enabled with a custom DCA configuration per parent-child relationship. This
also means that deletion can be enabled for parent-child-child relationships. A file must be
created for each level, which must include all levels "downward".

The following example covers the two-level hierarchy of MM tables ``mm_parent`` with ``mm_child``
and ``mm_child_child``. Depending on the Contao version, the DCA files must be placed in the
appropriate folder — in Contao 4.9, for example, in ``contao/dca`` (see the
`Contao documentation <https://docs.contao.org/dev/framework/dca/>`_).

.. code-block:: php
   :linenos:

    <?php
    // contao/dca/mm_parent.php
    $GLOBALS['TL_DCA']['mm_parent'] = [
        'dca_config' => [
            'data_provider'  => [
                'default' => [
                    'source' => 'mm_parent'
                ],
                'mm_child' => [
                    'source' => 'mm_child'
                ],
                'mm_child_child' => [
                    'source' => 'mm_child_child'
                ],
            ],
            'childCondition' => [
                [
                    'from'    => 'mm_parent',
                    'to'      => 'mm_child',
                    'setOn'   => [
                        [
                            'to_field'   => 'pid',
                            'from_field' => 'id',
                        ],
                    ],
                    'filter'  => [
                        [
                            'local'     => 'pid',
                            'remote'    => 'id',
                            'operation' => '=',
                        ],
                    ],
                    'inverse' => [
                        [
                            'local'     => 'pid',
                            'remote'    => 'id',
                            'operation' => '=',
                        ],
                    ]
                ],
                [
                    'from'    => 'mm_child',
                    'to'      => 'mm_child_child',
                    'setOn'   => [
                        [
                            'to_field'   => 'pid',
                            'from_field' => 'id',
                        ],
                    ],
                    'filter'  => [
                        [
                            'local'     => 'pid',
                            'remote'    => 'id',
                            'operation' => '=',
                        ],
                    ],
                    'inverse' => [
                        [
                            'local'     => 'pid',
                            'remote'    => 'id',
                            'operation' => '=',
                        ],
                    ]
                ],
            ],
        ]
    ];

.. code-block:: php
   :linenos:

    <?php
    // contao/dca/mm_child.php
    $GLOBALS['TL_DCA']['mm_child'] = [
        'dca_config' => [
            'data_provider'  => [
                'default' => [
                    'source' => 'mm_child'
                ],
                'mm_child_child' => [
                    'source' => 'mm_child_child'
                ],
            ],
            'childCondition' => [
                [
                    'from'    => 'mm_child',
                    'to'      => 'mm_child_child',
                    'setOn'   => [
                        [
                            'to_field'   => 'pid',
                            'from_field' => 'id',
                        ],
                    ],
                    'filter'  => [
                        [
                            'local'     => 'pid',
                            'remote'    => 'id',
                            'operation' => '=',
                        ],
                    ],
                    'inverse' => [
                        [
                            'local'     => 'pid',
                            'remote'    => 'id',
                            'operation' => '=',
                        ],
                    ]
                ],
            ],
        ]
    ];

Unfortunately, the relation with child tables is not yet supported in frontend editing (FEE),
so a custom deletion routine must be implemented there. To trigger it, the
`PostDeleteModelEvent <https://github.com/contao-community-alliance/dc-general/blob/61ffe2081323104b38ad951b2fbb3cb4b0f1a025/src/Event/PostDeleteModelEvent.php>`_
from DC_General could be used. Using the ID of the deleted model, all child records with the
same PID can be found and deleted.

If editing or deleting an MM item is done via a form, a deletion routine must be included there
as well.

.. |br| raw:: html

   <br />
