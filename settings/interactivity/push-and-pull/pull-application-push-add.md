# Send Push Notification to Mobile Device via pull.application.push.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`pull`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: administrator

The method `pull.application.push.add` sends a push notification to a mobile device within the application.

{% note info "" %}

This method works only in the context of the [application](../../app-installation/index.md).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **USER_ID** 
[`integer`](../../../api-reference/data-types.md) \| [`integer[]`](../../../api-reference/data-types.md) | Identifier of the user or an array of user identifiers to whom the push notification is sent.

`USER_ID` can be obtained via:
- the method [user.get](../../../api-reference/user/user-get.md)
- the method [user.current](../../../api-reference/user/user-current.md) for the current user ||
|| **TEXT**^*^ 
[`string`](../../../api-reference/data-types.md) | Text of the push notification ||
|| **AVATAR** 
[`string`](../../../api-reference/data-types.md) | URL of the image for the push notification ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Example of sending a push notification to application users, where:
- `USER_ID` — user identifier or an array of user identifiers
- `TEXT` — text of the push notification
- `AVATAR` — URL of the image for the push notification

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "USER_ID": [1, 2, 3],
        "TEXT": "Hello, world!",
        "AVATAR": "https://example.com/images/avatar.png",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/pull.application.push.add.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'pull.application.push.add',
    		{
    			USER_ID: [1, 2, 3],
    			TEXT: 'Hello, world!',
    			AVATAR: 'https://example.com/images/avatar.png'
    		}
    	);

    	const result = response.getData().result;
    	console.info(result);
    }
    catch (error)
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
                'pull.application.push.add',
                [
                    'USER_ID' => [1, 2, 3],
                    'TEXT' => 'Hello, world!',
                    'AVATAR' => 'https://example.com/images/avatar.png',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error sending push notification: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'pull.application.push.add',
        {
            USER_ID: [1, 2, 3],
            TEXT: 'Hello, world!',
            AVATAR: 'https://example.com/images/avatar.png'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    $result = CRest::call(
        'pull.application.push.add',
        [
            'USER_ID' => [1, 2, 3],
            'TEXT' => 'Hello, world!',
            'AVATAR' => 'https://example.com/images/avatar.png',
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1743495945,
        "finish": 1743495945.285066,
        "duration": 0.2850658893585205,
        "processing": 0.008597135543823242,
        "date_start": "2025-04-01T11:52:25+03:00",
        "date_finish": "2025-04-01T11:52:25+03:00",
        "operating_reset_at": 1743496545,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name** 
`type` | **Description** ||
|| **result** 
[`boolean`](../../../api-reference/data-types.md) | Indicator of successful method execution ||
|| **time** 
[`time`](../../../api-reference/data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **403**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Send push notifications available only for application authorization."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Send push notifications available only for application authorization. | Method call not in the context of application authorization ||
|| `400` | `ACCESS_ERROR` | You do not have access to send push notifications | User without administrator rights attempts to send a push notification ||
|| `400` | `TEXT_ERROR` | Text can't be empty | `TEXT` not provided or empty value passed ||
|| `400` | `EMPTY_APP_NAME` | For send push-notification application name can't be empty | Application name is not filled in ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../../interactivity/index.md)
- [{#T}](./pull-application-event-add.md)
- [{#T}](./pull-application-config-get.md)