# Delete List Element lists.element.delete

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: user with "Edit" access permission for the required list

The method `lists.element.delete` removes a list element.

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
|| **ELEMENT_ID***
[`integer`](../../data-types.md) | Identifier of the element.

The identifier can be obtained using the [lists.element.get](./lists-element-get.md) method ||
|| **ELEMENT_CODE***
[`string`](../../data-types.md) | Symbolic code of the element.

The code can be obtained using the [lists.element.get](./lists-element-get.md)

{% note info "" %}

At least one of the parameters must be specified: `ELEMENT_ID` or `ELEMENT_CODE`

{% endnote %} ||
|#

{% note info "" %}

When deleting an element, files from fields of type "File (Drive)" are removed from the drive only if they are not used anywhere else

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":47,"ELEMENT_ID":6999}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.element.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":47,"ELEMENT_ID":6999,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.element.delete
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'lists.element.delete',
            {
                IBLOCK_TYPE_ID: 'lists',
                IBLOCK_ID: 47,
                ELEMENT_ID: 6999,
            }
        );
        
        const result = response.getData().result;
        console.log('Deleted element with ID:', result);
        
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
                'lists.element.delete',
                [
                    'IBLOCK_TYPE_ID' => 'lists',
                    'IBLOCK_ID' => 47,
                    'ELEMENT_ID' => 6999
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting element: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'lists.element.delete',
        {
            IBLOCK_TYPE_ID: 'lists',
            IBLOCK_ID: 47,
            ELEMENT_ID: 6999
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
        'lists.element.delete',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => 47,
            'ELEMENT_ID' => 6999
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
        "start": 1763660361,
        "finish": 1763660361.659232,
        "duration": 0.6592319011688232,
        "processing": 0,
        "date_start": "2025-11-19T15:39:21+01:00",
        "date_finish": "2025-11-19T15:39:21+01:00",
        "operating_reset_at": 1763660961,
        "operating": 3.2583250999450684
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the element was successfully deleted ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_REQUIRED_PARAMETERS_MISSING",
    "error_description":"Required parameter is missing"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_REQUIRED_PARAMETERS_MISSING` | Required parameter `X` is missing | Required parameter not provided ||
|| `ERROR_IBLOCK_NOT_FOUND` | Iblock not found | Information block not found ||
|| `ERROR_ELEMENT_NOT_FOUND` | Element not found | Element with such `ID`/`CODE` not found ||
|| `ERROR_DELETE_ELEMENT` | — | Error deleting element ||
|| `ACCESS_DENIED` | Access denied | Insufficient rights to update the element ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./lists-element-add.md)
- [{#T}](./lists-element-get.md)
- [{#T}](./lists-element-update.md)
- [{#T}](./lists-element-get-file-url.md)