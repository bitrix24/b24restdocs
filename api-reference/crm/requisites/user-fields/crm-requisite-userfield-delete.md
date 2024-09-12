# Delete Custom Field of Requisite crm.requisite.userfield.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method deletes a custom field of the requisite.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the custom field. Can be obtained using the method [crm.requisite.userfield.list](./crm-requisite-userfield-list.md) ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":235}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.userfield.delete
    ```

- cURL (OAuth) 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":235,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.userfield.delete
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.requisite.userfield.delete",
        {
            id: 235
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
        'crm.requisite.userfield.delete',
        [
            'id' => 235
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
    "result": true,
    "time": {
        "start": 1717771689.639506,
        "finish": 1717771690.189804,
        "duration": 0.5502979755401611,
        "processing": 0.10051202774047852,
        "date_start": "2024-06-07T16:48:09+02:00",
        "date_finish": "2024-06-07T16:48:10+02:00",
        "operating": 0.1004788875579834
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of deleting the custom field:
- true — deleted
- false — not deleted
||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "The entity with ID '235' is not found."
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Code** | **Error Text** | **Description** ||
|| Empty string | `The entity with ID '235' is not found` | Custom field with the specified identifier not found ||
|| Empty string | `ID is not defined or invalid` | Identifier of the custom field is not specified or has an invalid value ||
|| Empty string | `Access denied` | Insufficient access permissions to delete the custom field ||
|| `ERROR_CORE` | `Fail to delete user field` | Failed to delete the custom field ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-userfield-add.md)
- [{#T}](./crm-requisite-userfield-get.md)
- [{#T}](./crm-requisite-userfield-list.md)
- [{#T}](./crm-requisite-userfield-update.md)