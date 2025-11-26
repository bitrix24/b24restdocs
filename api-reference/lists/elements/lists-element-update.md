# Update List Element lists.element.update

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Edit" permission for the required list

The method `lists.element.update` updates a list element.

{% note warning "" %}

The method completely overwrites the element. Fields whose values are not provided will be cleared.

{% endnote %}

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

The identifier can be obtained using the [lists.get](../lists/lists-get.md) method. ||
|| **IBLOCK_CODE*** 
[`string`](../../data-types.md) | Symbolic code of the information block.

The code can be obtained using the [lists.get](../lists/lists-get.md) method.

{% note info "" %}

At least one of the parameters must be specified: `IBLOCK_ID` or `IBLOCK_CODE`.

{% endnote %} ||
|| **ELEMENT_ID***
[`integer`](../../data-types.md) | Identifier of the element.

The identifier can be obtained using the [lists.element.get](./lists-element-get.md) method. ||
|| **ELEMENT_CODE***
[`string`](../../data-types.md) | Symbolic code of the element.

The code can be obtained using the [lists.element.get](./lists-element-get.md) method.

{% note info "" %}

At least one of the parameters must be specified: `ELEMENT_ID` or `ELEMENT_CODE`.

{% endnote %} ||
|| **FIELDS***
[`array`](../../data-types.md) | Array of fields.

[Detailed description](#parametr-fields) ||
|#

### FIELDS Parameter {#parametr-fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

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

More about working with files in the article [How to Update and Delete Files](../../files/how-to-update-files.md#listselementupdate-update-field-in-list)

||
|#

{% note info "" %}

You can get data about the list fields using the [lists.field.get](../fields/lists-field-get.md) method.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":47,"ELEMENT_ID":6999,"FIELDS":{"NAME":"Test Element (updated)","PROPERTY_951":["1269"]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.element.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":47,"ELEMENT_ID":6999,"FIELDS":{"NAME":"Test Element (updated)","PROPERTY_951":["1269"]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.element.update
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'lists.element.update',
            {
                IBLOCK_TYPE_ID: 'lists',
                IBLOCK_ID: 47,
                ELEMENT_ID: 6999,
                FIELDS: {
                    NAME: 'Test Element (updated)',
                    PROPERTY_951: ["1269"]
                }
            }
        );
        
        const result = response.getData().result;
        console.log('Updated element with ID:', result);
        processResult(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'lists.element.update',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_ID' => 47,
                    'ELEMENT_ID' => 6999,
                    'FIELDS' => [
                        'NAME' => 'Test Element (updated)',
                        'PROPERTY_951' => ["1269"]
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
        echo 'Error updating element: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'lists.element.update',
        {
            IBLOCK_TYPE_ID: 'lists',
            IBLOCK_ID: 47,
            ELEMENT_ID: 6999,
            FIELDS: {
                NAME: 'Test Element (updated)',
                PROPERTY_951: ["1269"]
            }
        },
        function(res) {
            if (res.error()) {
                console.error('Update error:', res.error());
            } else {
                console.log('Element successfully updated:', res.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'lists.element.update',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => 47,
            'ELEMENT_ID' => 6999,
            'FIELDS' => [
                'NAME' => 'Test Element (updated)',
                'PROPERTY_951' => ["1269"]
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
    "result": true,
    "time": {
        "start": 1763658078,
        "finish": 1763658078.767221,
        "duration": 0.7672209739685059,
        "processing": 0,
        "date_start": "2025-11-19T15:01:18+01:00",
        "date_finish": "2025-11-19T15:01:18+01:00",
        "operating_reset_at": 1763658678,
        "operating": 0.1465599536895752
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the element was successfully updated. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
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
|| `ERROR_REQUIRED_PARAMETERS_MISSING` | Required parameter `X` is missing | Required parameter not provided. ||
|| `ERROR_IBLOCK_NOT_FOUND` | Iblock not found | Information block not found. ||
|| `ERROR_ELEMENT_NOT_FOUND` | Element not found | Element with such `ID`/`CODE` not found. ||
|| `ERROR_UPDATE_ELEMENT` | — | Error updating the element. ||
|| `ERROR_ELEMENT_FIELD_VALUE` | Writing file values by ID is not supported | Field value validation error. ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to update the element. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./lists-element-add.md)
- [{#T}](./lists-element-get.md)
- [{#T}](./lists-element-delete.md)
- [{#T}](./lists-element-get-file-url.md)