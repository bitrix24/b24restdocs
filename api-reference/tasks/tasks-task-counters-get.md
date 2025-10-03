# Get User Counters tasks.task.counters.get

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.counters.get` retrieves task counter values for the specified user.

## Method Parameters

{% include [Footnote on parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **userId**
[`integer`](../data-types.md) | The identifier of the user for whom to retrieve counters.

By default, the current user is used.

The user identifier can be obtained using the [get user list](../user/user-get.md) method. ||
|| **groupId**
[`integer`](../data-types.md) | The identifier of the group for which to retrieve task counters.

Pass `0` or omit the parameter to consider all groups.

You can obtain the identifier using the [create new group](../sonet-group/sonet-group-create.md) method or the [get group list](../sonet-group/socialnetwork-api-workgroup-list.md) method. ||
|| **type**
[`string`](../data-types.md) | The role for which to retrieve counters. Possible roles:
- `view_all` — all roles
- `view_role_responsible` — responsible
- `view_role_accomplice` — accomplice
- `view_role_auditor` — auditor
- `view_role_originator` — originator
 
By default, the role `view_all` is used. ||
|#

## Code Examples

{% include [Footnote on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"userId":547,"groupId":0,"type":"view_all"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.counters.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"userId":547,"groupId":0,"type":"view_all","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.counters.get
    ```

- JS

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.counters.get',
            {
                userId: 547,
                groupId: 0,
                type: 'view_all',
            }
        );
        
        const result = response.getData().result;
        console.log('Task counters:', result);
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
                'tasks.task.counters.get',
                [
                    'userId' => 547,
                    'groupId' => 0,
                    'type' => 'view_all'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching task counters: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.task.counters.get',
        {
            userId: 547,
            groupId: 0,
            type: 'view_all'
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.counters.get',
        [
            'userId' => 547,
            'groupId' => 0,
            'type' => 'view_all'
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
        "expired": {
            "counter": 1,
            "code": 6291456
        },
        "new_comments": {
            "counter": 7,
            "code": 12582912
        }
    },
    "total": 1,
    "time": {
        "start": 1758868152,
        "finish": 1758868152.929809,
        "duration": 0.9298090934753418,
        "processing": 0,
        "date_start": "2025-09-26T09:29:12+02:00",
        "date_finish": "2025-09-26T09:29:12+02:00",
        "operating_reset_at": 1758868752,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | An object where the key is the counter name and the value is an object with [counter description](#counter).

Counter values:
- `new_comments` — unread comments
- `expired` — overdue tasks

Returns an empty array `"result":[]` if the user does not exist or does not have permission to retrieve the specified user's counters. ||
|| **total**
[`integer`](../data-types.md) | Currently not used. Always returns the value `1`. ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time. ||
|#

#### Counter Object {#counter}

#|
|| **Name**
`type` | **Description** ||
|| **counter**
[`integer`](../data-types.md) | Count. ||
|| **code**
[`integer`](../data-types.md) | Internal counter code. ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Group not found or access denied. (internal error)"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Argument #1 ($userId) must be of type int, string given, called in \/var\/www\/html\/bitrix\/modules\/tasks\/lib\/internals\/counter.php on line 78 (internal error) | The value in the `userId` parameter is of an incorrect type. ||
|| `0` | Group not found or access denied. (internal error) | The group does not exist or access is denied. ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./tasks-task-get.md)
- [{#T}](./tasks-task-list.md)