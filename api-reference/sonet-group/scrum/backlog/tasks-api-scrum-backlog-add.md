# Add backlog in Scrum tasks.api.scrum.backlog.add

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.api.scrum.backlog.add` adds a backlog in Scrum.

It may be necessary to create a backlog during import after creating the Scrum.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
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

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **groupId***
[`integer`](../../../data-types.md) | The identifier of the group for which the backlog is created.

The group identifier can be obtained when creating a new group [sonet_group.create](../../sonet-group-create.md) or when retrieving a list of existing groups [socialnetwork-api-workgroup-list.md](../../socialnetwork-api-workgroup-list.md) ||
|| **createdBy***
[`integer`](../../../data-types.md) | The identifier of the user who will create the backlog ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | The identifier of the user who will modify the backlog ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields": {"groupId": 1, "createdBy": 1}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.api.scrum.backlog.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields": {"groupId": 1, "createdBy": 1}, "auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.api.scrum.backlog.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'tasks.api.scrum.backlog.add',
    		{
    			"fields": {
    				"groupId": 125,
    				"createdBy": 6,
    			},
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.api.scrum.backlog.add',
                [
                    'fields' => [
                        'groupId'   => 125,
                        'createdBy' => 6,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding backlog: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.api.scrum.backlog.add',{
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
    'tasks.api.scrum.backlog.add',
        [
            'fields' => [
                'groupId' => 1,
                'createdBy' => 1,
            ],
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
    "result": {
        "id": 1,
        "groupId": 125,
        "createdBy": 6,
        "modifiedBy": 0
    },
    "time":{
        "start":1712137817.343984,
        "finish":1712137817.605804,
        "duration":0.26182007789611816,
        "processing":0.018325090408325195,
        "date_start":"2024-04-03T12:50:17+02:00",
        "date_finish":"2024-04-03T12:50:17+02:00"
    }
}
```

## Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | The identifier of the backlog ||
|| **groupId**
[`integer`](../../../data-types.md) | The identifier of the group for which the backlog was created ||
|| **createdBy**
[`integer`](../../../data-types.md) | The identifier of the user who created the backlog ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | The identifier of the user who modified the backlog ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":0,
    "error_description":"Access denied"
}
```
{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `0` | Backlog already added | The error occurs when trying to create a backlog while an active backlog already exists in the group ||
|| `0` | Access denied | Missing appropriate access permissions ||
|| `0` | createdBy user not found | The provided user identifier is invalid. For example, a user with such an identifier does not exist ||
|| `0` | modifiedBy user not found | The provided user identifier is invalid. For example, a user with such an identifier does not exist ||
|| `0` | Unknown error | Another error ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-api-scrum-backlog-update.md)
- [{#T}](./tasks-api-scrum-backlog-get.md)
- [{#T}](./tasks-api-scrum-backlog-delete.md)
- [{#T}](./tasks-api-scrum-backlog-get-fields.md)