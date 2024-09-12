# Get Order Binding Fields crm.orderentity.getFields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: online store manager

This method returns a list of available order binding fields. Each field is described as a field settings structure [crm_rest_field_description](../../data-types.md#crm_rest_field_description).

No parameters.

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.orderentity.getFields
    ```

- cURL (OAuth) 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/crm.orderentity.getFields?auth=**put_access_token_here**
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.orderentity.getFields",
        {},
    )
        .then(
            function(result)
            {
                if (result.error())
                {
                    console.error(result.error());
                }
                else
                {
                    console.log(result.data());
                }
            },
            function(error)
            {
                console.info(error);
            }
        );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.orderentity.getFields',
        []
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
        "orderEntity": {
            "orderId": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": true,
                "type": "integer"
            },
            "ownerId": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": true,
                "type": "integer"
            },
            "ownerTypeId": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": true,
                "type": "integer"
            }
        }
    },
    "time": {
        "start": 1718962657.018204,
        "finish": 1718962657.773002,
        "duration": 0.7547979354858398,
        "processing": 0.01193094253540039,
        "date_start": "2024-06-21T11:37:37+02:00",
        "date_finish": "2024-06-21T11:37:37+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **orderEntity**
[`object`](../../../data-types.md) | Object with a list of available fields in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field_N` is the identifier of the object [crm_orderentity](../../data-types.md#crm_orderentity), and `value` is an object of type [crm_rest_field_description](../../data-types.md#crm_rest_field_description) ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "200040300010",
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Code** | **Description** ||
|| `200040300010` | `Access Denied` 
Insufficient access permissions
||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-order-entity-add.md)
- [{#T}](./crm-order-entity-list.md)
- [{#T}](./crm-order-entity-delete-by-filter.md)