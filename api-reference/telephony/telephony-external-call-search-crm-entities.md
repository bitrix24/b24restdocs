# Find a Client in CRM by Phone Number telephony.externalCall.searchCrmEntities

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `telephony.externalCall.searchCrmEntities` returns CRM entities based on the client's phone number and the details of the responsible employee.

## Method Parameters

{% include [Note on Required Parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PHONE_NUMBER***
[`string`](../data-types.md) | The client's phone number ||
|#

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PHONE_NUMBER":"19062195047"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/telephony.externalCall.searchCrmEntities
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PHONE_NUMBER":"19062195047","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/telephony.externalCall.searchCrmEntities
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'telephony.externalCall.searchCrmEntities',
            {
                PHONE_NUMBER: '19062195047'
            }
        );
        
        const result = response.getData().result;
        console.log('CRM entities found:', result);
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
                'telephony.externalCall.searchCrmEntities',
                [
                    'PHONE_NUMBER' => '19062195047'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error searching CRM entities: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "telephony.externalCall.searchCrmEntities",
        {
            PHONE_NUMBER: '19062195047'
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
        'telephony.externalCall.searchCrmEntities',
        [
            'PHONE_NUMBER' => '19062195047'
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
    "result": [
        {
        "CRM_ENTITY_TYPE": "CONTACT",
        "CRM_ENTITY_ID": 651,
        "ASSIGNED_BY_ID": 99,
        "NAME": "John Doe",
        "ASSIGNED_BY": {
            "ID": "99",
            "TIMEMAN_STATUS": "CLOSED",
            "USER_PHONE_INNER": null,
            "WORK_PHONE": null,
            "PERSONAL_PHONE": null,
            "PERSONAL_MOBILE": "19062195047"
        }
        },
        {
        "CRM_ENTITY_TYPE": "COMPANY",
        "CRM_ENTITY_ID": 643,
        "ASSIGNED_BY_ID": 99,
        "NAME": "Daisy LLC",
        "ASSIGNED_BY": {
            "ID": "99",
            "TIMEMAN_STATUS": "CLOSED",
            "USER_PHONE_INNER": null,
            "WORK_PHONE": null,
            "PERSONAL_PHONE": null,
            "PERSONAL_MOBILE": "19062195047"
        }
        }
    ],
    "time": {
        "start": 1772808159,
        "finish": 1772808159.397228,
        "duration": 0.3972280025482178,
        "processing": 0,
        "date_start": "2026-03-06T17:42:39+02:00",
        "date_finish": "2026-03-06T17:42:39+02:00",
        "operating_reset_at": 1772808759,
        "operating": 0.25037074089050293
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | An array of found CRM entities ||
|| **CRM_ENTITY_TYPE**
[`string`](../data-types.md) | Type of the CRM entity.

Possible values:
- `CONTACT` — contact
- `LEAD` — lead
- `COMPANY` — company ||
|| **CRM_ENTITY_ID**
[`integer`](../data-types.md) | Identifier of the CRM entity ||
|| **ASSIGNED_BY_ID**
[`integer`](../data-types.md) | Identifier of the responsible employee ||
|| **NAME**
[`string`](../data-types.md) | Name or full name of the found CRM entity ||
|| **ASSIGNED_BY**
[`object`](../data-types.md) | Data of the [responsible employee](#result-assigned-by) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### ASSIGNED_BY Object {#result-assigned-by}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Identifier of the employee ||
|| **TIMEMAN_STATUS**
[`string`](../data-types.md) | Status of the employee's working time.

Possible values:
- `OPENED` — workday started
- `CLOSED` — workday ended
- `PAUSED` — break
- `EXPIRED` — workday expired
- `UNAVAILABLE` — time tracking is disabled for the employee
- `NOT_INSTALLED` — time tracking module is not installed ||
|| **USER_PHONE_INNER**
[`string`](../data-types.md) | Employee's internal number ||
|| **WORK_PHONE**
[`string`](../data-types.md) | Employee's work phone ||
|| **PERSONAL_PHONE**
[`string`](../data-types.md) | Employee's personal phone ||
|| **PERSONAL_MOBILE**
[`string`](../data-types.md) | Employee's mobile phone ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "PHONE_NUMBER is empty"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_CORE` | PHONE_NUMBER is empty | The required parameter `PHONE_NUMBER` was not provided ||
|| `ERROR_CORE` | CRM is not installed. | The CRM module is not installed on the account ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./telephony-external-call-register.md)
- [{#T}](./telephony-external-call-show.md)
- [{#T}](./telephony-external-call-hide.md)
- [{#T}](./telephony-external-call-finish.md)