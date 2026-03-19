# Delete Open Channel imopenlines.config.delete

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: user with permission to modify open channels

The method `imopenlines.config.delete` removes an open channel.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONFIG_ID***
[`integer`](../../data-types.md) | Identifier of the open channel.

You can obtain the identifier of the open channel when [creating an open channel](./imopenlines-config-add.md) or by using the [method to get the list of open channels](./imopenlines-config-list-get.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "CONFIG_ID": 15
      }' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.config.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "CONFIG_ID": 15,
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imopenlines.config.delete
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imopenlines.config.delete',
            {
                CONFIG_ID: 15
            }
        );

        const result = response.getData().result;
        console.log(result);
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
                'imopenlines.config.delete',
                [
                    'CONFIG_ID' => 15,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.config.delete',
        {
            CONFIG_ID: 15
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
        'imopenlines.config.delete',
        [
            'CONFIG_ID' => 15,
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1741688626.157568,
        "finish": 1741688626.261411,
        "duration": 0.10384297370910645,
        "processing": 0.04578208923339844,
        "date_start": "2025-03-11T10:30:26+01:00",
        "date_finish": "2025-03-11T10:30:26+01:00",
        "operating_reset_at": 1741689226,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns:
- `true` — if the channel was successfully deleted
- `false` — if the channel with the specified `CONFIG_ID` does not exist ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "CONFIG_ID_EMPTY",
    "error_description": "Config ID can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CONFIG_ID_EMPTY` | Config ID can't be empty | The `CONFIG_ID` parameter is missing or incorrectly provided ||
|| `403` | `CONFIG_WRONG_USER_PERMISSION` | Permission denied | Insufficient rights to delete the channel ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./imopenlines-config-add.md)
- [{#T}](./imopenlines-config-update.md)
- [{#T}](./imopenlines-config-get.md)
- [{#T}](./imopenlines-config-list-get.md)
- [{#T}](./imopenlines-config-path-get.md)