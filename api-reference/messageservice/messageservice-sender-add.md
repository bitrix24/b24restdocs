# Register an SMS Provider or Message Provider messageservice.sender.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`messageservice`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `messageservice.sender.add` registers a new message provider.

This method works only in the context of an [application](../../settings/app-installation/index.md).

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../data-types.md) | Provider code.

Allowed characters: `a-z`, `A-Z`, `0-9`, `.`, `-`, `_` ||
|| **TYPE***
[`string`](../data-types.md) | Provider type.

Supported value: `SMS` ||
|| **HANDLER***
[`string`](../data-types.md) | Application handler URL that is called when sending a message ||
|| **NAME***
[`string` \| `object`](../data-types.md) | Provider name.

Can be a string or an associative array of localized strings like:

```js
'NAME': {
    'de': 'Anbietername',
    'en': 'provider name',
    ...
},
```
 ||
|| **DESCRIPTION**
[`string` \| `object`](../data-types.md) | Provider description.

Can be a string or an associative array of localized strings like:

```js
'DESCRIPTION': {
    'de': 'Anbieterbeschreibung',
    'en': 'provider description',
    ...
},
```
||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"provider1","TYPE":"SMS","HANDLER":"https://provider.example/api/handler","NAME":"Provider 1","DESCRIPTION":"Main SMS provider","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/messageservice.sender.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'messageservice.sender.add',
            {
                CODE: 'provider1',
                TYPE: 'SMS',
                HANDLER: 'https://provider.example/api/handler',
                NAME: 'Provider 1',
                DESCRIPTION: 'Main SMS provider'
            }
        );

        const result = response.getData().result;
        console.log(result);
    }
    catch (error)
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
                'messageservice.sender.add',
                [
                    'CODE' => 'provider1',
                    'TYPE' => 'SMS',
                    'HANDLER' => 'https://provider.example/api/handler',
                    'NAME' => 'Provider 1',
                    'DESCRIPTION' => 'Main SMS provider',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding sender: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'messageservice.sender.add',
        {
            CODE: 'provider1',
            TYPE: 'SMS',
            HANDLER: 'https://provider.example/api/handler',
            NAME: 'Provider 1',
            DESCRIPTION: 'Main SMS provider'
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
        'messageservice.sender.add',
        [
            'CODE' => 'provider1',
            'TYPE' => 'SMS',
            'HANDLER' => 'https://provider.example/api/handler',
            'NAME' => 'Provider 1',
            'DESCRIPTION' => 'Main SMS provider',
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
    "result": true,
    "time": {
        "start": 1742895600,
        "finish": 1742895600.845505,
        "duration": 0.845505952835083,
        "processing": 0,
        "date_start": "2025-03-25T10:00:00+02:00",
        "date_finish": "2025-03-25T10:00:00+02:00",
        "operating_reset_at": 1742896200,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | `true` if the provider was successfully registered ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "ERROR_SENDER_VALIDATION_FAILURE",
    "error_description": "Empty sender code!"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_SENDER_VALIDATION_FAILURE` | `Empty data!` | Empty parameter set ||
|| `ERROR_SENDER_VALIDATION_FAILURE` | `Empty sender code!` | Required parameter `CODE` not provided ||
|| `ERROR_SENDER_VALIDATION_FAILURE` | `Wrong sender code!` | `CODE` contains invalid characters ||
|| `ERROR_SENDER_VALIDATION_FAILURE` | `Empty sender NAME!` | Required parameter `NAME` not provided ||
|| `ERROR_SENDER_VALIDATION_FAILURE` | `Empty sender message TYPE!` | Required parameter `TYPE` not provided ||
|| `ERROR_SENDER_VALIDATION_FAILURE` | `Unknown sender message TYPE!` | Unsupported `TYPE` provided (only `SMS` is allowed) ||
|| `ERROR_SENDER_ALREADY_INSTALLED` | `Sender already installed!` | A provider with this `CODE` is already registered for the current application ||
|| `ERROR_SENDER_ADD_FAILURE` | `Sender save error!` | Error saving the provider ||
|| `ACCESS_DENIED` | `Access denied!` | Method was invoked by a non-administrator ||
|| `ACCESS_DENIED` | `Application context required` | Method called outside of application context ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./messageservice-sender-add.md)
- [{#T}](./messageservice-sender-update.md)
- [{#T}](./messageservice-sender-list.md)
- [{#T}](./messageservice-sender-delete.md)
- [{#T}](./messageservice-message-status-update.md)