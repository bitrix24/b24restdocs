# Delete Record from Search History im.search.last.delete

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.search.last.delete` removes a dialog from the last search history.

This method was designed for the previous version of the chat. In the current version of the chat M1, it works, but the results are not displayed in the interface.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **DIALOG_ID***
[`string`](../../data-types.md) | Identifier for the message object: user or chat.

Supported formats:
- `USER_ID` — user identifier, which can be obtained via [user.get](../../user/user-get.md) or [user.search](../../user/user-search.md)
- `chatXXX`, where `XXX` is the chat identifier, which can be obtained via [im.recent.get](../im-recent-get.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat17"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.search.last.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat17","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.search.last.delete
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.search.last.delete', {
        DIALOG_ID: 'chat17',
      });

      const { result } = response.getData();
      console.log(result);
    } catch (error) {
      console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'im.search.last.delete',
            [
                'DIALOG_ID' => 'chat17',
            ]
        );

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
    BX24.callMethod(
        'im.search.last.delete',
        {
            DIALOG_ID: 'chat17',
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.search.last.delete',
        [
            'DIALOG_ID' => 'chat17',
        ]
    );

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
        "start": 1772449081,
        "finish": 1772449081.887056,
        "duration": 0.8870561122894287,
        "processing": 0,
        "date_start": "2026-03-02T13:58:01+01:00",
        "date_finish": "2026-03-02T13:58:01+01:00",
        "operating_reset_at": 1772449681,
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the record was deleted, otherwise `false` ||
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

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `DIALOG_ID_EMPTY` | Dialog ID can't be empty | An invalid `DIALOG_ID` was provided ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-search-chat-list.md)
- [{#T}](./im-search-department-list.md)
- [{#T}](./im-search-user-list.md)
- [{#T}](./im-search-last-add.md)
- [{#T}](./im-search-last-get.md)