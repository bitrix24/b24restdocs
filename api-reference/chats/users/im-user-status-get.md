# Get User Status im.user.status.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.user.status.get` returns the status of the current user.

{% note info "" %}

The method returns the status that was set using the [im.user.status.set](./im-user-status-set.md) method.

{% endnote %}

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.user.status.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.user.status.get
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.user.status.get', {});
      const { result } = response.getData();
      console.log(result);
    } catch (error) {
      console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call('im.user.status.get', []);

        $result = $response->getResponseData()->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            var_dump($result->data());
        }
    } catch (Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('im.user.status.get', {}, function(result) {
        if (result.error()) {
            console.error(result.error().ex);
        } else {
            console.log(result.data());
        }
    });
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call('im.user.status.get', []);

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        var_dump($result['result']);
    }
    ```
{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": "dnd",
    "time": {
        "start": 1760000000.0,
        "finish": 1760000000.05,
        "duration": 0.05,
        "processing": 0.02,
        "date_start": "2026-03-02T09:30:00+01:00",
        "date_finish": "2026-03-02T09:30:00+01:00",
        "operating_reset_at": 1760030000,
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`string`](../../data-types.md) 
[`boolean`](../../data-types.md) | The current status of the user. 

Allowed values: 
- `online` — online
- `dnd` — do not disturb
- `away` — away
- `break` — on break

If the status is not found, `false` is returned ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

{% note info "" %}

In the interface of the new messenger, only the `online` status is displayed. The statuses `dnd`, `away`, and `break` can be set using the method, but they are not shown in the interface.

[Bitrix24 Chat: New Messenger](https://helpdesk.bitrix24.com/open/25661218/)

{% endnote %}

## Error Handling

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-user-get.md)
- [{#T}](./im-user-list-get.md)
- [{#T}](./im-user-status-set.md)
- [{#T}](./im-user-status-idle-start.md)
- [{#T}](./im-user-status-idle-end.md)