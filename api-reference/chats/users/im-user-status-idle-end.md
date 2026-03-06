# Disable Automatic Status "Away" im.user.status.idle.end

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.user.status.idle.end` disables the automatic "Away" status for the current user.

This method was designed for the previous version of the chat. In the current version of the chat M1, it works, but the results are not displayed in the interface.

{% note tip "User Documentation" %}

- [Bitrix24 Chat: New Messenger](https://helpdesk.bitrix24.com/open/25661218/)

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
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.user.status.idle.end
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.user.status.idle.end
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.user.status.idle.end', {});
      const { result } = response.getData();
      console.log(result);
    } catch (error) {
      console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call('im.user.status.idle.end', []);

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
    BX24.callMethod('im.user.status.idle.end', {}, function(result) {
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

    $result = CRest::call('im.user.status.idle.end', []);

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
    "result": true,
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
[`boolean`](../../data-types.md) | Returns `true` if the "Away" status is disabled ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-user-get.md)
- [{#T}](./im-user-list-get.md)
- [{#T}](./im-user-status-set.md)
- [{#T}](./im-user-status-get.md)
- [{#T}](./im-user-status-idle-start.md)