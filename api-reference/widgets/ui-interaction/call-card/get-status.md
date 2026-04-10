# Get Call Status getStatus

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The `getStatus` method returns the current data of the call card.

{% note info "" %}

The method operates within the application context in the `CALL_CARD` placement.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PLACEMENT***
[`string`](../../../data-types.md) | The name of the interface command.

For this method — `getStatus` ||
|| **PARAMS***
[`object`](../../../data-types.md) | The parameters object for the command.

For this method, an empty object is passed: `{}` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"getStatus","PARAMS":{}}' \
    "https://**put_your_bitrix24_address**/rest/placement.call?auth=**put_access_token_here**"
    ```

- JS

    ```js
    BX24.placement.call('getStatus', {}, function (result) {
        console.log(result);
    });
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'placement.call',
                [
                    'PLACEMENT' => 'getStatus',
                    'PARAMS' => []
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'placement.call',
        {
            PLACEMENT: 'getStatus',
            PARAMS: {}
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
        'placement.call',
        [
            'PLACEMENT' => 'getStatus',
            'PARAMS' => (object)[]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

```json
{
    "CALL_ID": "E45D40253D1C2D2F.1774588815.822533",
    "PHONE_NUMBER": "+19999999666",
    "LINE_NUMBER": "reg151083",
    "LINE_NAME": "",
    "CRM_ENTITY_TYPE": "CONTACT",
    "CRM_ENTITY_ID": 797,
    "CRM_ACTIVITY_ID": "",
    "CRM_BINDINGS": [
        {
        "ENTITY_TYPE": "DEAL",
        "ENTITY_ID": 4615
        },
        {
        "ENTITY_TYPE": "COMPANY",
        "ENTITY_ID": 643
        }
    ],
    "CALL_DIRECTION": "outgoing",
    "CALL_STATE": "idle",
    "CALL_LIST_MODE": false
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **CALL_ID**
[`string`](../../../data-types.md) | The identifier of the call ||
|| **PHONE_NUMBER**
[`string`](../../../data-types.md) | The client's number ||
|| **LINE_NUMBER**
[`string`](../../../data-types.md) | The line number ||
|| **LINE_NAME**
[`string`](../../../data-types.md) | The name of the line ||
|| **CRM_ENTITY_TYPE**
[`string`](../../../data-types.md) | The type of the current CRM object ||
|| **CRM_ENTITY_ID**
[`integer`](../../../data-types.md) | The identifier of the current CRM object ||
|| **CRM_ACTIVITY_ID**
[`integer`](../../../data-types.md) | The identifier of the CRM activity ||
|| **CRM_BINDINGS**
[`object[]`](../../../data-types.md) | The bindings of the call to CRM objects [(detailed description)](#crm_bindings) ||
|| **CALL_DIRECTION**
[`string`](../../../data-types.md) | The direction of the call.

Possible values:

- `incoming` — incoming call
- `outgoing` — outgoing call
- `callback` — callback ||
|| **CALL_STATE**
[`string`](../../../data-types.md) | The state of the call.

Possible values:

- `idle` — no connection
- `connecting` — establishing connection
- `connected` — connection established ||
|| **CALL_LIST_MODE**
[`boolean`](../../../data-types.md) | Indicator of the dialing mode ||
|#

### CRM_BINDINGS Parameter {#crm_bindings}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_TYPE**
[`string`](../../../data-types.md) | The type of the CRM object ||
|| **ENTITY_ID**
[`integer`](../../../data-types.md) | The identifier of the CRM object ||
|#

## Error Handling

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Application context required"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `WRONG_AUTH_TYPE` | Application context required | The method was called outside the application context in the `CALL_CARD` placement ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./disable-auto-close.md)
- [{#T}](./enable-auto-close.md)
- [{#T}](./call-card-entity-changed.md)
- [{#T}](./call-card-before-close.md)
- [{#T}](./call-card-call-state-changed.md)