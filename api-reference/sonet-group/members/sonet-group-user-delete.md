# Remove users from group sonet_group.user.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sonet`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sonet_group.user.delete` removes participants from a workgroup or project.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **GROUP_ID***
[`integer`](../../data-types.md) | Identifier of the workgroup or project.

The identifier can be obtained using the [sonet_group.get](../sonet-group-get.md) method. ||
|| **USER_ID***
[`integer/array`](../../data-types.md) | Identifier of the participant.

The identifier can be obtained using the [sonet_group.user.get](./sonet-group-user-get.md) method. ||
|#

{% note info "" %}

You cannot remove the owner of the group or project and the scrum master.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"GROUP_ID":69,"USER_ID":[1271,1272]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sonet_group.user.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"GROUP_ID":69,"USER_ID":[1271,1272],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sonet_group.user.delete
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'sonet_group.user.delete',
            {
                GROUP_ID: 69,
                USER_ID: [1271, 1272]
            }
        );
        
        const result = response.getData().result;
        console.log('Users deleted from group:', result);
        
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
                'sonet_group.user.delete',
                [
                    'GROUP_ID' => 69,
                    'USER_ID' => [1271, 1272]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting users from group: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sonet_group.user.delete',
        {
            GROUP_ID: 69,
            USER_ID: [1271, 1272]
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
        'sonet_group.user.delete',
        [
            'GROUP_ID' => 69,
            'USER_ID' => [1271, 1272]
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
    "result": ["1271", "1272"],
    "time": {
        "start": 1773810480,
        "finish": 1773810480.5882,
        "duration": 0.5882,
        "processing": 0.2973,
        "date_start": "2026-03-18T08:48:00+02:00",
        "date_finish": "2026-03-18T08:48:00+02:00",
        "operating_reset_at": 1773811080,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array of user identifiers that were successfully deleted.

An empty array means that there was an attempt to remove the owner of the group or project. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request. ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "No permissions to update users role"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| — | `Wrong group ID` | An incorrect `GROUP_ID` was provided. ||
|| — | `No permissions to update users role` | Insufficient rights to remove participants. ||
|| — | `Wrong user IDs` | An empty or incorrect `USER_ID` was provided. ||
|| — | `Socialnetwork group not found` | Group or project with the specified `GROUP_ID` not found. ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sonet-group-user-invite.md)
- [{#T}](./sonet-group-user-request.md)
- [{#T}](./sonet-group-user-add.md)
- [{#T}](./sonet-group-user-update.md)
- [{#T}](./sonet-group-user-get.md)