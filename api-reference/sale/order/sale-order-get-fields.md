# Get Order Fields sale.order.getfields

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.order.getfields` retrieves the available fields of an order.

No parameters.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.order.getfields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.order.getfields
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.order.getfields", {},
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.order.getfields',
        []
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
    "result": {
        "order": {
            "accountNumber": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "string"
            },
            "additionalInfo": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "affiliateId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "canceled": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "comments": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "companyId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "currency": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": true,
                "type": "string"
            },
            "dateCanceled": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "datetime"
            },
            "dateInsert": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "datetime"
            },
            "dateLock": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "datetime"
            },
            "dateMarked": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "datetime"
            },
            "dateStatus": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "datetime"
            },
            "dateUpdate": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "datetime"
            },
            "deducted": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "char"
            },
            "discountValue": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "double"
            },
            "empCanceledId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "empMarkedId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "empStatusId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "externalOrder": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "id": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "integer"
            },
            "id1c": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "lid": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": true,
                "type": "string"
            },
            "lockedBy": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "marked": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "orderTopic": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "payed": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "char"
            },
            "personTypeId": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": true,
                "type": "integer"
            },
            "personTypeXmlId": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "string"
            },
            "price": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "double"
            },
            "reasonCanceled": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "reasonMarked": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "recountFlag": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "recurringId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "responsibleId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "statusId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "statusXmlId": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "string"
            },
            "taxValue": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "double"
            },
            "updated1c": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "userDescription": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "userId": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "version": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "integer"
            },
            "version1c": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "xmlId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            }
        }
    },
    "time": {
        "start": 1712825736.172681,
        "finish": 1712825736.838046,
        "duration": 0.6653649806976318,
        "processing": 0.03579592704772949,
        "date_start": "2024-04-11T11:55:36+03:00",
        "date_finish": "2024-04-11T11:55:36+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **order**
[`object`](../../data-types.md) | Object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`. Where `field` is the identifier of the object [sale_order](../data-types.md), and `value` is an object of type [rest_field_description](../data-types.md#rest_field_description) ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":0,
    "error_description":"error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read available order fields ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./sale-order-add.md)
- [{#T}](./sale-order-update.md)
- [{#T}](./sale-order-get.md)
- [{#T}](./sale-order-list.md)
- [{#T}](./sale-order-delete.md)