# Add Product Property crm.product.property.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator, user with the "Allow to modify settings" access permission in CRM

{% note warning "Method development halted" %}

The method `crm.product.property.add` is still operational, but there is a more relevant alternative [catalog.productProperty.add](../../../catalog/product-property/catalog-product-property-add.md).

{% endnote %}

The method `crm.product.property.add` creates a new product property.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields**
[`array`](../../../data-types.md) | Field values for creating a product property.

To find out the required field format, execute the method [crm.product.property.fields](./crm-product-property-fields.md) and check the format of the returned field values ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

### Example 1

{% list tabs %}

- JS

    ```js
    // Creating a custom property of type S:HTML
    function addPropertyExample(catalogId)
    {
        BX24.callMethod(
            "crm.product.property.add",
            {
                fields: {
                    "ACTIVE": "Y",
                    "IBLOCK_ID": catalogId,
                    "NAME": "Property - HTML/Text",
                    "SORT": 500,
                    "DEFAULT_VALUE": {
                        "TYPE": "html",
                        "TEXT": "<u><b>Delicious</b> \"<span style=\"color: #00a650;\">African bananas</span>\"</u>"
                    },
                    "USER_TYPE_SETTINGS": {
                        "HEIGHT": 300
                    },
                    "USER_TYPE": "HTML",
                    "PROPERTY_TYPE": "S"
                }
            },
            function(result)
            {
                if(result.error())
                    console.error(result.error());
                else
                    console.dir(result.data());
            }
        );
    }

    function getCatalogId()
    {
        BX24.callMethod(
            "crm.catalog.list",
            {filter: {"ORIGINATOR_ID": "", "ORIGIN_ID": ""}},
            function(result)
            {
                if(result.error())
                {
                    console.error(result.error());
                }
                else
                {
                    var catalogId = 0;
                    if (result.total() !== 1)
                    {
                        catalogId = 0
                    }
                    else
                    {
                        data = result.data();
                        if (data && data instanceof Array && typeof(data[0]) === "object" && data[0]["ID"])
                        {
                            catalogId = parseInt(data[0]["ID"]);
                        }
                    }
                    if (catalogId <= 0)
                    {
                        console.error("Failed to retrieve the CRM product catalog ID");
                    }
                    else
                    {
                        addPropertyExample(catalogId);
                    }
                }
            }
        );
    }

    function start()
    {
        getCatalogId();
    }

    start();
    ```

- PHP

    ```php
    require_once('crest.php');

    // Function to add a custom property
    function addPropertyExample($catalogId) {
        $result = CRest::call(
            'crm.product.property.add',
            [
                'fields' => [
                    'ACTIVE' => 'Y',
                    'IBLOCK_ID' => $catalogId,
                    'NAME' => 'Property - HTML/Text',
                    'SORT' => 500,
                    'DEFAULT_VALUE' => [
                        'TYPE' => 'html',
                        'TEXT' => '<u><b>Delicious</b> "<span style="color: #00a650;">African bananas</span>"</u>'
                    ],
                    'USER_TYPE_SETTINGS' => [
                        'HEIGHT' => 300
                    ],
                    'USER_TYPE' => 'HTML',
                    'PROPERTY_TYPE' => 'S'
                ]
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
    }

    // Function to get catalog ID
    function getCatalogId() {
        $result = CRest::call(
            'crm.catalog.list',
            ['filter' => ['ORIGINATOR_ID' => '', 'ORIGIN_ID' => '']]
        );

        if ($result['error']) {
            echo 'Error: ' . $result['error_description'];
        } else {
            $catalogId = 0;
            if ($result['total'] !== 1) {
                $catalogId = 0;
            } else {
                $data = $result['result'];
                if ($data && is_array($data) && isset($data[0]['ID'])) {
                    $catalogId = intval($data[0]['ID']);
                }
            }
            if ($catalogId <= 0) {
                echo "Failed to retrieve the CRM product catalog ID";
            } else {
                addPropertyExample($catalogId);
            }
        }
    }

    // Start the process
    getCatalogId();
    ```

{% endlist %}

### Example 2

{% list tabs %}

- JS
  
    ```js
    function addPropertyExample(catalogId)
    {
        BX24.callMethod(
            "crm.product.property.add",
            {
                fields: {
                    "ACTIVE": "Y",
                    "IBLOCK_ID": catalogId,
                    "NAME": "Property - List",
                    "SORT": 510,
                    "MULTIPLE": "Y",
                    "ROW_COUNT": 7,
                    "LIST_TYPE": "L",
                    "PROPERTY_TYPE": "L",
                    "VALUES": {
                        "n0": {
                            "ID": "n0",
                            "VALUE": "List value 1",
                            "SORT": 100,
                            "DEF": "Y"
                        },
                        "n1": {
                            "ID": "n1",
                            "VALUE": "List value 2",
                            "SORT": 200,
                            "DEF": "N"
                        },
                        "n2": {
                            "ID": "n2",
                            "VALUE": "List value 3",
                            "SORT": 300,
                            "DEF": "Y"
                        },
                        "n3": {
                            "ID": "n3",
                            "VALUE": "List value 4",
                            "SORT": 400,
                            "DEF": "N"
                        },
                        "n4": {
                            "ID": "n4",
                            "VALUE": "List value 5",
                            "SORT": 500,
                            "DEF": "N"
                        },
                        "n5": {
                            "ID": "n5",
                            "VALUE": "List value 6",
                            "SORT": 600,
                            "DEF": "N"
                        },
                        "n6": {
                            "ID": "n6",
                            "VALUE": "List value 7",
                            "SORT": 700,
                            "DEF": "N"
                        },
                        "n7": {
                            "ID": "n7",
                            "VALUE": "List value 8",
                            "SORT": 800,
                            "DEF": "N"
                        }
                    }
                }
            },
            function(result)
            {
                if(result.error())
                    console.error(result.error());
                else
                    console.dir(result.data());
            }
        );
    }

    function getCatalogId()
    {
        BX24.callMethod(
            "crm.catalog.list",
            {filter: {"ORIGINATOR_ID": "", "ORIGIN_ID": ""}},
            function(result)
            {
                if(result.error())
                {
                    console.error(result.error());
                }
                else
                {
                    var catalogId = 0;
                    if (result.total() !== 1)
                    {
                        catalogId = 0
                    }
                    else
                    {
                        data = result.data();
                        if (data && data instanceof Array && typeof(data[0]) === "object" && data[0]["ID"])
                        {
                            catalogId = parseInt(data[0]["ID"]);
                        }
                    }
                    if (catalogId <= 0)
                    {
                        console.error("Failed to retrieve the CRM product catalog ID");
                    }
                    else
                    {
                        addPropertyExample(catalogId);
                    }
                }
            }
        );
    }

    function start()
    {
        getCatalogId();
    }

    start();
    ```

- PHP

    ```php
    require_once('crest.php');

    function addPropertyExample($catalogId) {
        $result = CRest::call(
            'crm.product.property.add',
            [
                'fields' => [
                    'ACTIVE' => 'Y',
                    'IBLOCK_ID' => $catalogId,
                    'NAME' => 'Property - List',
                    'SORT' => 510,
                    'MULTIPLE' => 'Y',
                    'ROW_COUNT' => 7,
                    'LIST_TYPE' => 'L',
                    'PROPERTY_TYPE' => 'L',
                    'VALUES' => [
                        'n0' => [
                            'ID' => 'n0',
                            'VALUE' => 'List value 1',
                            'SORT' => 100,
                            'DEF' => 'Y'
                        ],
                        'n1' => [
                            'ID' => 'n1',
                            'VALUE' => 'List value 2',
                            'SORT' => 200,
                            'DEF' => 'N'
                        ],
                        'n2' => [
                            'ID' => 'n2',
                            'VALUE' => 'List value 3',
                            'SORT' => 300,
                            'DEF' => 'Y'
                        ],
                        // Continue for other values...
                    ]
                ]
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
    }

    function getCatalogId() {
        $result = CRest::call(
            'crm.catalog.list',
            ['filter' => ['ORIGINATOR_ID' => '', 'ORIGIN_ID' => '']]
        );

        if (isset($result['error'])) {
            echo 'Error: ' . $result['error_description'];
        } else {
            $catalogId = 0;
            if (count($result['result']) !== 1) {
                echo "Failed to retrieve the CRM product catalog ID";
            } else {
                $data = $result['result'];
                if (isset($data[0]['ID'])) {
                    $catalogId = intval($data[0]['ID']);
                    addPropertyExample($catalogId);
                }
            }
        }
    }

    getCatalogId();
    ```

{% endlist %}