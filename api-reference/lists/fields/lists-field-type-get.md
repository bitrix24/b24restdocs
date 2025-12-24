# Get Available Field Types for Universal List lists.field.type.get

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Full access" permission for the required list

The method `lists.field.type.get` returns a list of available field types for the list.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **IBLOCK_TYPE_ID*** 
[`string`](../../data-types.md) | Identifier of the information block type. Possible values: 
- `lists` — list information block type 
- `bitrix_processes` — processes information block type 
- `lists_socnet` — group lists information block type 

The identifier can be obtained using the method [lists.get.iblock.type.id](../lists/lists-get-iblock-type-id.md) ||
|| **IBLOCK_ID*** 
[`integer`](../../data-types.md) | Identifier of the information block.

The identifier can be obtained using the method [lists.get](../lists/lists-get.md) ||
|| **IBLOCK_CODE*** 
[`string`](../../data-types.md) | Symbolic code of the information block.

The code can be obtained using the method [lists.get](../lists/lists-get.md)

{% note info "" %}

At least one of the parameters must be specified: `IBLOCK_ID` or `IBLOCK_CODE`

{% endnote %} ||
|| **FIELD_ID** 
[`integer`](../../data-types.md) | Field identifier. For a custom field, it appears as `PROPERTY_PropertyId`. For a system field, it is its symbolic code.

If an identifier of an existing field in the list is specified, its type will be included in the result. For system fields, this means that their type will be displayed, even though they cannot be added again ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":"123"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.field.type.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":"123","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.field.type.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'lists.field.type.get',
            {
                IBLOCK_TYPE_ID: 'lists',
                IBLOCK_ID: '123',
            }
        );
        
        const result = response.getData().result;
        console.log('Data:', result);
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
                'lists.field.type.get',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_ID' => '123'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'lists.field.type.get', 
        {
            'IBLOCK_TYPE_ID': 'lists',
            'IBLOCK_ID': '123'
        }, 
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'lists.field.type.get',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => '123'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "SORT": "Sorting",
        "ACTIVE_FROM": "Start of activity",
        "ACTIVE_TO": "End of activity",
        "PREVIEW_PICTURE": "Preview image",
        "PREVIEW_TEXT": "Preview text",
        "DETAIL_PICTURE": "Detail image",
        "DETAIL_TEXT": "Detail text",
        "CREATED_BY": "Created by",
        "TIMESTAMP_X": "Modification date",
        "MODIFIED_BY": "Modified by",
        "S": "String",
        "N": "Number",
        "L": "List",
        "F": "File",
        "G": "Binding to sections",
        "E": "Binding to elements",
        "S:Date": "Date",
        "S:DateTime": "Date/Time",
        "S:HTML": "HTML/text",
        "E:EList": "Binding to elements as a list",
        "N:Sequence": "Counter",
        "S:ECrm": "Binding to CRM elements",
        "S:Money": "Money",
        "S:DiskFile": "File (Disk)",
        "S:employee": "Binding to employee"
    },
    "time": {
        "start": 1765379410,
        "finish": 1765379410.123019,
        "duration": 0.12301898002624512,
        "processing": 0,
        "date_start": "2025-12-10T16:10:10+01:00",
        "date_finish": "2025-12-10T16:10:10+01:00",
        "operating_reset_at": 1765380010,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array of available field types for the list ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"ERROR_REQUIRED_PARAMETERS_MISSING",
    "error_description":"Required parameter `X` is missing"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_REQUIRED_PARAMETERS_MISSING` | Required parameter `X` is missing | Required parameter not provided ||
|| `ERROR_IBLOCK_NOT_FOUND` | Iblock not found | Information block not found ||
|| `ACCESS_DENIED` | Access denied | Insufficient rights to read ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./lists-field-add.md)
- [{#T}](./lists-field-update.md)
- [{#T}](./lists-field-get.md)
- [{#T}](./lists-field-delete.md)