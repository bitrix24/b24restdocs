# Get Notification Type Schema im.notify.schema.get

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.schema.get` returns the schema of available notification types by modules.

## Method Parameters

No parameters

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.notify.schema.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.notify.schema.get
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.notify.schema.get', {});
      const { result } = response.getData();
      console.log(result);
    } catch (error) {
      console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call('im.notify.schema.get', []);
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
    BX24.callMethod('im.notify.schema.get', {}, function(result) {
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

    $result = CRest::call('im.notify.schema.get', []);

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
    "result": {
        "tasks": {
            "name": "Tasks",
            "module_id": "tasks",
            "list": [
                {
                    "id": "tasks|task_update",
                    "name": "Task Updates"
                }
            ]
        }
    },
    "time": {
        "start": 1760000000.0,
        "finish": 1760000000.1,
        "duration": 0.1,
        "processing": 0.04,
        "date_start": "2026-03-03T10:00:00+01:00",
        "date_finish": "2026-03-03T10:00:00+01:00",
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
[`object`](../../data-types.md) | The notification schema object, with the top-level key being `MODULE_ID`. 

The structure of the module object is described in detail [below](#module-object) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### Object \{MODULE_ID\} {#module-object}

#|
|| **Name**
`Type` | **Description** ||
|| **NAME**
[`string`](../../data-types.md) | The name of the module ||
|| **MODULE_ID**
[`string`](../../data-types.md) | The module identifier ||
|| **LIST**
[`array`](../../data-types.md) | A list of notification types for the module. 

The structure of the list item is described in detail [below](#list-item-object) ||
|#

### LIST Item {#list-item-object}

#|
|| **Name**
`Type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | The identifier of the notification type in the format ```MODULE|EVENT``` ||
|| **NAME**
[`string`](../../data-types.md) | The name of the notification type ||
|#

## Error Handling

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-notify.md)
- [{#T}](./im-notify-personal-add.md)
- [{#T}](./im-notify-system-add.md)
- [{#T}](./im-notify-get.md)
- [{#T}](./im-notify-read-list.md)
- [{#T}](./im-notify-read.md)
- [{#T}](./im-notify-read-all.md)
- [{#T}](./im-notify-answer.md)
- [{#T}](./im-notify-confirm.md)
- [{#T}](./im-notify-delete.md)
- [{#T}](./im-notify-history-search.md)