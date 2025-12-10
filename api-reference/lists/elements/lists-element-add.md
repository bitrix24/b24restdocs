# Create a list element lists.element.add

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Add" or "Edit" access permission for the required list

The method `lists.element.add` creates a list element.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **IBLOCK_TYPE_ID***
[`string`](../../data-types.md) | Identifier of the information block. Possible values: 
- `lists` — list information block type 
- `bitrix_processes` — processes information block type 
- `lists_socnet` — group lists information block type ||
|| **IBLOCK_ID***
[`integer`](../../data-types.md) | Identifier of the information block.

The identifier can be obtained using the [lists.get](../lists/lists-get.md) method ||
|| **IBLOCK_CODE*** 
[`string`](../../data-types.md) | Symbolic code of the information block.

The code can be obtained using the [lists.get](../lists/lists-get.md) method

{% note info "" %}

At least one of the parameters must be specified: `IBLOCK_ID` or `IBLOCK_CODE`

{% endnote %} ||
|| **ELEMENT_CODE***
[`string`](../../data-types.md) | Symbolic code of the element ||
|| **FIELDS***
[`array`](../../data-types.md) | Array of fields.

[Detailed description](#parametr-fields) ||
|| **IBLOCK_SECTION_ID**
[`integer`](../../data-types.md) | Identifier of the section to which the element is added.

If the parameter is not passed, the element is created at the root of the list. The default value is `0`.

The identifier can be obtained using the [lists.section.get](../sections/lists-section-get.md) method ||
|| **LIST_ELEMENT_URL**
[`string`](../../data-types.md) | Template address to the list elements.

Supports replacements: `#list_id#`, `#section_id#`, `#element_id#`, `#group_id#` ||
|#

### FIELDS Parameter {#parametr-fields}

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME***
[`string`](../../data-types.md) | Name of the element ||
|| **PROPERTY_PropertyId** | Custom properties.

Any property of the element can be configured as multiple. For multiple properties, pass an array, even if there is only one value. 
   
To pass a value in a File type field, specify:
- for File type — [base64](../../files/how-to-upload-files.md) or an array with the name and base64
- for File type (Drive) — file identifier from Drive

||
|#

{% note info "" %}

You can get data about the list fields using the [lists.field.get](../fields/lists-field-get.md) method

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":47,"ELEMENT_CODE":"test_element","LIST_ELEMENT_URL":"#list_id#/element/#section_id#/#element_id#/","FIELDS":{"NAME":"Test Element","PROPERTY_951":["1269","1271"],"PROPERTY_1003":"2024-12-31 23:59:59"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.element.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":47,"ELEMENT_CODE":"test_element","LIST_ELEMENT_URL":"#list_id#/element/#section_id#/#element_id#/","FIELDS":{"NAME":"Test Element","PROPERTY_951":["1269","1271"],"PROPERTY_1003":"2024-12-31 23:59:59"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.element.add
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod(
            'lists.element.add',
            {
                IBLOCK_TYPE_ID: 'lists',
                IBLOCK_ID: 47,
                ELEMENT_CODE: 'test_element',
                LIST_ELEMENT_URL: '#list_id#/element/#section_id#/#element_id#/',
                FIELDS: {
                    NAME: 'Test Element',
                    PROPERTY_951: ["1269", "1271"],
                    PROPERTY_1003: "2024-12-31 23:59:59"
                }
            }
        );

        const result = response.getData().result;
        console.log('Created element with ID:', result);
        processResult(result);
    } catch (error) {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'lists.element.add',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_ID' => 47,
                    'ELEMENT_CODE' => 'test_element',
                    'LIST_ELEMENT_URL' => '#list_id#/element/#section_id#/#element_id#/',
                    'FIELDS' => [
                        'NAME' => 'Test Element',
                        'PROPERTY_951' => ["1269", "1271"],
                        'PROPERTY_1003' => "2024-12-31 23:59:59"
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding element: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'lists.element.add',
        {
            IBLOCK_TYPE_ID: 'lists',
            IBLOCK_ID: 47,
            ELEMENT_CODE: 'test_element',
            LIST_ELEMENT_URL: '#list_id#/element/#section_id#/#element_id#/',
            FIELDS: {
                NAME: 'Test Element',
                PROPERTY_951: ["1269", "1271"], // Custom property of type String (multiple)
                PROPERTY_1003: "2024-12-31 23:59:59" // Custom property of type Date/Time
            }
        },
        function(res) {
            if (res.error()) {
                console.error(res.error());
            } else {
                console.log(res.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'lists.element.add',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => 47,
            'ELEMENT_CODE' => 'test_element',
            'LIST_ELEMENT_URL' => '#list_id#/element/#section_id#/#element_id#/',
            'FIELDS' => [
                'NAME' => 'Test Element',
                'PROPERTY_951' => ["1269", "1271"],
                'PROPERTY_1003' => "2024-12-31 23:59:59"
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 6999,
    "time": {
        "start": 1763654360,
        "finish": 1763654360.814629,
        "duration": 0.814629077911377,
        "processing": 0,
        "date_start": "2025-11-19T13:59:20+02:00",
        "date_finish": "2025-11-19T13:59:20+02:00",
        "operating_reset_at": 1763654960,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the created element ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_ELEMENT_FIELD_VALUE",
    "error_description":"Writing file values by ID is not supported"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_REQUIRED_PARAMETERS_MISSING` | Required parameter `X` is missing | Required parameter not provided ||
|| `ERROR_IBLOCK_NOT_FOUND` | Iblock not found | Information block not found ||
|| `ERROR_ELEMENT_ALREADY_EXISTS` | Element already exists | An element with this `CODE` already exists ||
|| `ERROR_ADD_ELEMENT` | — | Error adding element ||
|| `ERROR_ELEMENT_FIELD_VALUE` | Writing file values by ID is not supported | Field value validation error ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to add element ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./lists-element-update.md)
- [{#T}](./lists-element-get.md)
- [{#T}](./lists-element-delete.md)
- [{#T}](./lists-element-get-file-url.md)