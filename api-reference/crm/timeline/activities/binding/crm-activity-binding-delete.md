# Delete the deal's connection with the CRM entity crm.activity.binding.delete

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

The method `crm.activity.binding.delete` removes the connection of a deal with a CRM entity. The deletion of the deal's binding is only possible for entities that the current user has edit access to.

If the deal is bound to only one entity, this binding cannot be deleted.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **activityId***  
[`integer`](../../../../data-types.md) | Integer identifier of the deal in the timeline, for example `999` ||
|| **entityTypeId***  
[`integer`](../../../../data-types.md) | [Integer identifier of the CRM object type](../../../data-types.md#object_type) to which the deal's binding is being removed, for example `2` for a deal ||
|| **entityId***  
[`integer`](../../../../data-types.md) | Integer identifier of the CRM entity to which the deal's binding is being removed, for example `1`  ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"activityId":999, "entityTypeId":2, "entityId": 1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.binding.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"activityId":999, "entityTypeId":2, "entityId": 1, "auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.binding.delete
    ```

- JS

    ```javascript
    BX24.callMethod(
        'crm.activity.binding.delete',
        {
            activityId: 999, // Deal ID
            entityTypeId: 2, // CRM object type ID
            entityId: 1 // CRM entity ID
        },
        function(result) {
            if (result.error()) {
                console.error('Error:', result.error()); 
            } else {
                console.log('Result:', result.data()); 
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.binding.delete',
        [
            'activityId' => 999, // Deal ID
            'entityTypeId' => 2, // CRM object type ID
            'entityId' => 1 // CRM entity ID
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
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../../data-types.md) | Result of the operation. Returns `true` if the binding was successfully deleted, otherwise — `false` ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Entity not found"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `100` | Required fields are missing ||
|| `NOT_FOUND` | Entity not found ||
|| `OWNER_NOT_FOUND` | Entity owner not found ||
|| `ACCESS_DENIED` | Insufficient permissions to perform the operation ||
|| `BINDING_NOT_FOUND` | The deal is not bound to this entity ||
|| `LAST_BINDING_CANNOT_BE_DELETED` | Cannot delete the only binding of the deal to the entity ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-binding-list.md)
- [{#T}](./crm-activity-binding-add.md)
- [{#T}](./crm-activity-binding-move.md)