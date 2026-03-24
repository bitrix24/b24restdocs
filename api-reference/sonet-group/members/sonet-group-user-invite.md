# Invite users to group sonet_group.user.invite

> Scope: [`sonet`](../../scopes/permissions.md)
>
> Who can perform the method: a user with the permission to Invite to a group or project

The method `sonet_group.user.invite` sends invitations to users in a workgroup or project.

## Method parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **GROUP_ID***
[`integer`](../../data-types.md) | Identifier of the workgroup or project.

The identifier can be obtained using the [sonet_group.get](../sonet-group-get.md) method. ||
|| **USER_ID***
[`integer/array`](../../data-types.md) | User identifier.

The identifier can be obtained using the [user.get](../../user/user-get.md) method. ||
|| **MESSAGE**
[`string`](../../data-types.md) | Invitation text. ||
|#

## Code examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"GROUP_ID":69,"USER_ID":1271,"MESSAGE":"Join the project"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sonet_group.user.invite
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"GROUP_ID":69,"USER_ID":1271,"MESSAGE":"Join the project","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sonet_group.user.invite
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'sonet_group.user.invite',
            {
                GROUP_ID: 69,
                USER_ID: 1271,
                MESSAGE: 'Join the project',
            }
        );
        
        const result = response.getData().result;
        console.log('User invited to group:', result);
        
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
                'sonet_group.user.invite',
                [
                    'GROUP_ID' => 69,
                    'USER_ID' => 1271,
                    'MESSAGE' => 'Join the project'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error inviting user to group: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sonet_group.user.invite',
        {
            GROUP_ID: 69,
            USER_ID: 1271,
            MESSAGE: 'Join the project'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sonet_group.user.invite',
        [
            'GROUP_ID' => 69,
            'USER_ID' => 1271,
            'MESSAGE' => 'Join the project'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response handling

HTTP status: **200**

```json
{
    "result": ["1271"],
    "time": {
        "start": 1773846932,
        "finish": 1773846933.077318,
        "duration": 1.0773179531097412,
        "processing": 0,
        "date_start": "2026-03-18T18:15:32+02:00",
        "date_finish": "2026-03-18T18:15:33+02:00",
        "operating_reset_at": 1773847533,
        "operating": 0
    }
}
```

### Returned data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | An array of user identifiers that were successfully invited.

An empty array means that none of the provided users could be invited. For example, the user is already a member of the group or the current user does not have permission to invite. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

## Error handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Wrong user IDs"
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible error codes

#|
|| **Code** | **Description** | **Value** ||
|| — | `Wrong group ID` | An incorrect `GROUP_ID` was provided. ||
|| — | `Wrong user IDs` | An empty or incorrect `USER_ID` was provided. ||
|| — | `Socialnetwork group not found` | The group or project was not found or is not accessible to the current user. ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue exploring

- [{#T}](./sonet-group-user-request.md)
- [{#T}](./sonet-group-user-add.md)
- [{#T}](./sonet-group-user-update.md)
- [{#T}](./sonet-group-user-get.md)
- [{#T}](./sonet-group-user-delete.md)