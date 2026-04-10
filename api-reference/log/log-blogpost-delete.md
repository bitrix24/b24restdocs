# Delete News Feed Message log.blogpost.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`log`](../scopes/permissions.md)
>
> Who can execute the method: administrator or message author

The method `log.blogpost.delete` removes a message from the News Feed.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **POST_ID***
[`integer`](../data-types.md) | Identifier of the message.

You can obtain the identifier using the [log.blogpost.get](./log-blogpost-get.md) method ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"POST_ID":211}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/log.blogpost.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"POST_ID":211,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/log.blogpost.delete
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'log.blogpost.delete',
            {
                POST_ID: 211
            }
        );
        
        const result = response.getData().result;
        console.log(result);
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
                'log.blogpost.delete',
                [
                    'POST_ID' => 211
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting blog post: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'log.blogpost.delete',
        {
            POST_ID: 211
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
        'log.blogpost.delete',
        [
            'POST_ID' => 211
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
        "start": 1773751532,
        "finish": 1773751532.632761,
        "duration": 0.6327610015869141,
        "processing": 0,
        "date_start": "2026-03-17T15:45:32+01:00",
        "date_finish": "2026-03-17T15:45:32+01:00",
        "operating_reset_at": 1773752132,
        "operating": 0.1264798641204834
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | `true` if the message was deleted ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "100",
    "error_description": "Wrong post ID"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | `Wrong post ID` | Invalid `POST_ID` ||
|| `0` | `Blog module is not installed.` | The `blog` module is not installed ||
|| `403` | `You do not have permission to delete this message` | Insufficient permissions to delete the message ||
|| `0` | `Error deleting message` | Internal error while deleting the message ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./log-blogpost-add.md)
- [{#T}](./log-blogpost-update.md)
- [{#T}](./log-blogpost-get.md)
- [{#T}](./log-blogpost-share.md)
- [{#T}](./log-blogpost-getusers-important.md)