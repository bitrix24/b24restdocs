# Change Role of Group Participants sonet_group.user.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sonet`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sonet_group.user.update` changes the role of participants in a workgroup or project.

To change the owner, use the method [sonet_group.setowner](../sonet-group-setowner.md)

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **GROUP_ID***
[`integer`](../../data-types.md) | Identifier of the workgroup or project.

The identifier can be obtained using the method [sonet_group.get](../sonet-group-get.md) ||
|| **USER_ID***
[`integer/array`](../../data-types.md) | Identifier of the participant.

The identifier can be obtained using the method [sonet_group.user.get](./sonet-group-user-get.md) ||
|| **ROLE***
[`string`](../../data-types.md) | Code of the new participant role.

Possible values:
- `E` — moderator
- `K` — participant
||
|#

{% note info "" %}

The user's role will not be updated if the user is the owner of the group or project.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"GROUP_ID":69,"USER_ID":[1271,1272],"ROLE":"E"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sonet_group.user.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"GROUP_ID":69,"USER_ID":[1271,1272],"ROLE":"E","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sonet_group.user.update
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'sonet_group.user.update',
            {
                GROUP_ID: 69,
                USER_ID: [1271, 1272],
                ROLE: 'E'
            }
        );
        
        const result = response.getData().result;
        console.log('Users updated in group:', result);
        
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
                'sonet_group.user.update',
                [
                    'GROUP_ID' => 69,
                    'USER_ID' => [1271, 1272],
                    'ROLE' => 'E'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating group users: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sonet_group.user.update',
        {
            GROUP_ID: 69,
            USER_ID: [1271],
            ROLE: 'E'
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
        'sonet_group.user.update',
        [
            'GROUP_ID' => 69,
            'USER_ID' => [1271, 1272],
            'ROLE' => 'E'
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
        "start": 1773810360,
        "finish": 1773810360.6451,
        "duration": 0.6451,
        "processing": 0.3112,
        "date_start": "2026-03-18T08:46:00+02:00",
        "date_finish": "2026-03-18T08:46:00+02:00",
        "operating_reset_at": 1773810960,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array of user identifiers for which the role was successfully updated.

An empty array means that there was an attempt to change the role of the group or project owner. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Incorrect role code"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| — | `Wrong group ID` | An incorrect `GROUP_ID` was provided. ||
|| — | `No permissions to update users role` | Insufficient rights to change participant roles. ||
|| — | `Incorrect role code` | An unsupported value for `ROLE` was provided. ||
|| — | `Wrong user IDs` | An empty or incorrect `USER_ID` was provided. ||
|| — | `Socialnetwork group not found` | The group or project with the specified `GROUP_ID` was not found. ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sonet-group-user-invite.md)
- [{#T}](./sonet-group-user-request.md)
- [{#T}](./sonet-group-user-add.md)
- [{#T}](./sonet-group-user-get.md)
- [{#T}](./sonet-group-user-delete.md)