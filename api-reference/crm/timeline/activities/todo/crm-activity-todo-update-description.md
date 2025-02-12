# Update Description of Universal Deal crm.activity.todo.updateDescription

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: a user with edit access permission for the CRM entity to which the deal is linked

The method `crm.activity.todo.updateDescription` changes the description in the universal deal.

## Method Parameters

{% include [Footnote on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../../data-types.md) | Identifier of the deal being updated, for example `999` ||
|| **ownerTypeId***
[`integer`](../../../../data-types.md) | [Identifier of the CRM object type](../../../data-types.md#object_type) to which the deal is linked, for example `2` for a deal ||
|| **ownerId***
[`integer`](../../../../data-types.md) | Identifier of the CRM entity to which the deal is linked, for example `1` ||
|| **value***
[`string`](../../../../data-types.md) | New description of the deal ||
|#

## Code Examples

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"ownerTypeId":2,"ownerId":1,"value":"New deal description"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.todo.updateDescription
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":999,"ownerTypeId":2,"ownerId":1,"value":"New deal description","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.todo.updateDescription
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.activity.todo.updateDescription",
        {
            id: 999,
            ownerTypeId: 2,
            ownerId: 1,
            value: 'New deal description'
        }, 
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.todo.updateDescription',
        [
            'id' => 999,
            'ownerTypeId' => 2,
            'ownerId' => 1,
            'value' => 'New deal description'
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
        "id": 999
    },
    "time": {
       "start": 1724068028.331234,
        "finish": 1724068028.726591,
        "duration": 0.3953571319580078,
        "processing": 0.13033390045166016,
        "date_start": "2025-01-21T13:47:08+02:00",
        "date_finish": "2025-01-21T13:47:08+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | On success, returns an object containing the identifier of the updated deal `id`, on error = `null` ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Not found."
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `100` | Required fields not provided ||
|| `NOT_FOUND` | CRM entity not found ||
|| `ACCESS_DENIED` | Insufficient permissions to perform the operation ||
|| `OWNER_NOT_FOUND` | Owner of the entity not found ||
|| `CAN_NOT_UPDATE_COMPLETED_TODO` | Completed deal cannot be modified ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-todo-add.md)
- [{#T}](./crm-activity-todo-update.md)
- [{#T}](./crm-activity-todo-update-deadline.md)
- [{#T}](./crm-activity-todo-update-color.md)
- [{#T}](./crm-activity-todo-update-responsible-user.md)