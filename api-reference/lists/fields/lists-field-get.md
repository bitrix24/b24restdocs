# Get Field Parameters or List of Fields `lists.field.get`

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "Read" access permission for the required list

The method `lists.field.get` returns data about a field or a list of fields.

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
[`string`](../../data-types.md) | Identifier of the field. For a custom field, it appears as `PROPERTY_PropertyId`. For a system field, it is its symbolic code.

If not specified, all fields of the list are returned ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":"123","FIELD_ID":"PROPERTY_1151"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.field.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":"123","FIELD_ID":"PROPERTY_1151","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.field.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'lists.field.get',
            {
                IBLOCK_TYPE_ID: 'lists',
                IBLOCK_ID: '123',
                FIELD_ID: 'PROPERTY_1151',
            }
        );
        
        const result = response.getData().result;
        console.log('Fetched field data:', result);
        
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
                'lists.field.get',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_ID' => '123',
                    'FIELD_ID' => 'PROPERTY_1151'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching field: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'lists.field.get',
        {
            'IBLOCK_TYPE_ID': 'lists', 
            'IBLOCK_ID': '123',        
            'FIELD_ID': 'PROPERTY_1151'
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
        'lists.field.get',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => '123',
            'FIELD_ID' => 'PROPERTY_1151'
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
        "L": {
        "FIELD_ID": "PROPERTY_1151",
        "SORT": 50,
        "NAME": "Task Status",
        "IS_REQUIRED": "N",
        "MULTIPLE": "N",
        "DEFAULT_VALUE": "1685",
        "TYPE": "L",
        "PROPERTY_TYPE": "L",
        "PROPERTY_USER_TYPE": false,
        "CODE": "PROJECT",
        "ID": "1151",
        "LINK_IBLOCK_ID": null,
        "ROW_COUNT": "1",
        "COL_COUNT": "30",
        "USER_TYPE_SETTINGS": null,
        "SETTINGS": {
            "SHOW_ADD_FORM": "Y",
            "SHOW_EDIT_FORM": "Y",
            "ADD_READ_ONLY_FIELD": "N",
            "EDIT_READ_ONLY_FIELD": "Y",
            "SHOW_FIELD_PREVIEW": "N"
        },
        "DISPLAY_VALUES_FORM": {
            "1669": "Planning",
            "1671": "In Active Work",
            "1673": "Testing",
            "1675": "Completed",
            "1677": "Deferred",
            "1679": "Archive"
        }
        }
    },
    "time": {
        "start": 1765375929,
        "finish": 1765375929.696936,
        "duration": 0.6969358921051025,
        "processing": 0,
        "date_start": "2025-12-10T12:12:09+01:00",
        "date_finish": "2025-12-10T12:12:09+01:00",
        "operating_reset_at": 1765376529,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Data of the field or an array of fields.

An empty array means that there are no fields in the list with the specified `FIELD_ID` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
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
|| `ACCESS_DENIED` | Access denied | Insufficient rights for reading ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./lists-field-add.md)
- [{#T}](./lists-field-update.md)
- [{#T}](./lists-field-delete.md)
- [{#T}](./lists-field-type-get.md)