# Update Backlog tasks.api.scrum.backlog.update

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates the backlog.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Backlog identifier.

You can obtain it using the method [tasks.api.scrum.backlog.get](./tasks-api-scrum-backlog-get.md) ||
|| **fields***
[`object`](../../../data-types.md) | An object containing records about the group and user (detailed description provided below) in the following structure:

```js
"fields": {
    "groupId": value,
    "createdBy": value,
    "modifiedBy": value,
}    
```
||
|#

### fields Parameter

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **groupId**
[`integer`](../../../data-types.md) | Identifier of the group to which the backlog belongs.

The group identifier can be obtained when creating a new group [sonet_group.create](../../sonet-group-create.md) or when retrieving a list of existing groups [socialnetwork-api-workgroup-list.md](../../socialnetwork-api-workgroup-list.md) ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the user who will create the backlog ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | Identifier of the user who will modify the backlog ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1, "fields": {"groupId": 125, "createdBy": 6}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.api.scrum.backlog.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1, "fields": {"groupId": 125, "createdBy": 6}, "auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.api.scrum.backlog.update
    ```

- JS

    ```js
    BX24.callMethod(
        'tasks.api.scrum.backlog.update',
        {
            "id": 1,
            "fields": {
                "groupId": 125,
                "createdBy": 6,
            },
        },
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
        'tasks.api.scrum.backlog.update',
        [
            'id' => 1,
            'fields' => [
                'groupId' => 125,
                'createdBy' => 6,
            ],
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
        "id": 1,
        "groupId": 125,
        "createdBy": 6,
        "modifiedBy": 1
    },
    "time": {
        "start": 1712137817.343984,
        "finish": 1712137817.605804,
        "duration": 0.26182007789611816,
        "processing": 0.018325090408325195,
        "date_start": "2024-04-03T12:50:17+02:00",
        "date_finish": "2024-04-03T12:50:17+02:00"
    }
}
```

## Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Backlog identifier ||
|| **groupId**
[`integer`](../../../data-types.md) | Identifier of the group for which the backlog was created ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the user who created the backlog ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | Identifier of the user who modified the backlog ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 0,
    "error_description": "Access denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `100` | Could not find value for parameter {fields} | Required parameter `fields` not provided ||
|| `0` | Backlog not found | Invalid backlog identifier provided ||
|| `0` | Access denied | Insufficient access permissions ||
|| `0` | Unknown error | Other error ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-api-scrum-backlog-add.md)
- [{#T}](./tasks-api-scrum-backlog-get.md)
- [{#T}](./tasks-api-scrum-backlog-delete.md)
- [{#T}](./tasks-api-scrum-backlog-get-fields.md)