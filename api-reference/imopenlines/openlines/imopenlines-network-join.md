# Connect an External Open Channel to the Account imopenlines.network.join

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imopenlines.network.join` connects an external open channel from another Bitrix24 to the current Bitrix24.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../../data-types.md) | The code of the open channel as a string of 32 characters.

You can find the channel code in the detail form of the connected open channel in Bitrix24 Network on the Contact Center page.

{% note tip "User Documentation" %}

- [Contact Center: Bitrix24.Network](https://helpdesk.bitrix24.com/open/19124962/)

{% endnote %}

||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "CODE": "ab515f5d85a8b844d484f6ea75a2e494"
      }' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.network.join
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "CODE": "ab515f5d85a8b844d484f6ea75a2e494",
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imopenlines.network.join
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imopenlines.network.join',
            {
                CODE: 'ab515f5d85a8b844d484f6ea75a2e494'
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
                'imopenlines.network.join',
                [
                    'CODE' => 'ab515f5d85a8b844d484f6ea75a2e494',
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
        'imopenlines.network.join',
        {
            CODE: 'ab515f5d85a8b844d484f6ea75a2e494'
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
        'imopenlines.network.join',
        [
            'CODE' => 'ab515f5d85a8b844d484f6ea75a2e494',
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 603,
    "time": {
        "start": 1773735146,
        "finish": 1773735146.911103,
        "duration": 0.9111030101776123,
        "processing": 0,
        "date_start": "2026-03-17T11:12:26+02:00",
        "date_finish": "2026-03-17T11:12:26+02:00",
        "operating_reset_at": 1773735746,
        "operating": 0.44127893447875977
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | The identifier of the network bot for the connected external open channel. 

If the channel was previously connected, the method returns the existing identifier ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "LINE_NOT_FOUND",
    "error_description": "Line not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CODE` | You entered an invalid code | Invalid code in parameter `CODE`, expected a string of 32 characters ||
|| `400` | `IMBOT_ERROR` | Module IMBOT is not installed | The imbot module is not installed ||
|| `400` | `LINE_NOT_FOUND` | Line not found | Open channel not found ||
|| `400` | `INACTIVE` | Openline is inactive | The open channel is currently unavailable ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./imopenlines-network-message-add.md)