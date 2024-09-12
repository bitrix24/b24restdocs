# Delete Requisite and Related Objects crm.requisite.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method deletes a requisite and all related objects (connections with other entities, addresses, bank details).

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the requisite. 

The identifier can be obtained using the [crm.requisite.list](./crm-requisite-list.md) method. ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":27}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":27,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.delete
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.requisite.delete",
        {
            id: 27
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.delete',
        [
            'id' => 27
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1717166469.477791,
        "finish": 1717166470.500321,
        "duration": 1.0225298404693604,
        "processing": 0.3063521385192871,
        "date_start": "2024-05-31T16:41:09+02:00",
        "date_finish": "2024-05-31T16:41:10+02:00",
        "operating": 0.30628108978271484
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Returns the value:

- `true` — requisite deleted
- `false` — requisite not deleted

||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Response

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The Requisite with ID '57' is not found"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Code** | **Error Text** | **Description** ||
|| Empty string | The Requisite with ID '57' is not found | The requisite with the specified identifier was not found ||
|| Empty string | ID is not defined or invalid. | The requisite identifier is not specified or has an invalid value ||
|| Empty string | Access denied. | Insufficient access permissions to delete the requisite ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-requisite-add.md)
- [{#T}](./crm-requisite-update.md)
- [{#T}](./crm-requisite-get.md)
- [{#T}](./crm-requisite-list.md)
- [{#T}](./crm-requisite-fields.md)