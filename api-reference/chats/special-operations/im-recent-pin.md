# Pin a Chat at the Top of the im.recent List

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: chat participant

The method `im.recent.pin` pins or unpins a conversation at the top of the user's chat list.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **DIALOG_ID*** 
[`string`](../../data-types.md) | Identifier of the chat in the format:

- `chatXXX` — chat
- `sgXXX` — group or project chat
- `XXX` — personal chat user ID

The chat identifier can be obtained using the [im.chat.get](../im-chat-get.md) method. The user ID can be retrieved using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods ||
|| **PIN**
[`string`](../../data-types.md) | Pin or unpin the conversation:

- `Y` — pin the conversation at the top of the list
- `N` — unpin the conversation

Default is `Y` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat1489","PIN":"Y"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.recent.pin
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DIALOG_ID":"chat1489","PIN":"Y","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.recent.pin
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.recent.pin',
            {
                DIALOG_ID: 'chat1489',
                PIN: 'Y'
            }
        );

        const result = response.getData().result;
        console.log('Pinned dialog:', result);

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
                'im.recent.pin',
                [
                    'DIALOG_ID' => 'chat1489',
                    'PIN'       => 'Y'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error pinning dialog: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.recent.pin',
        {
            DIALOG_ID: 'chat1489',
            PIN: 'Y'
        },
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
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.recent.pin',
        [
            'DIALOG_ID' => 'chat1489',
            'PIN'       => 'Y'
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
    "result": true,
    "time": {
        "start": 1772519500,
        "finish": 1772519500.234567,
        "duration": 0.234567165374756,
        "processing": 0,
        "date_start": "2026-03-03T09:31:40+01:00",
        "date_finish": "2026-03-03T09:31:40+01:00",
        "operating_reset_at": 1772520100,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the conversation is successfully pinned or unpinned ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "DIALOG_ID_EMPTY",
    "error_description": "Dialog ID can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `DIALOG_ID_EMPTY` | Dialog ID can’t be empty | The `DIALOG_ID` parameter is not provided, is empty, or is in an incorrect format ||
|| `400` | `ACCESS_ERROR` | You don’t have permission to pin this chat | The user does not have access to the chat ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-recent-unread.md)
- [{#T}](./im-dialog-read-all.md)
- [{#T}](./im-chat-mute.md)
- [{#T}](./im-recent-hide.md)