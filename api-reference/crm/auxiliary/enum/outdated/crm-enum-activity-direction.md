# Get Activity Direction Enumeration Elements crm.enum.activitydirection

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "Method development has been discontinued" %}

The method `crm.enum.activitydirection` continues to function, but it is considered deprecated in favor of the more current methods [crm.activity.*](../../../timeline/activities/index.md). A more relevant alternative is the methods [crm.activity.todo.*](../../../timeline/activities/todo/index.md). 

{% endnote %}

The method `crm.enum.activitydirection` returns activity directions for the `DIRECTION` field of [deals, emails, and calls](../../../timeline/activities/index.md). 

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.enum.activitydirection",
        {},
        function(result) {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.enum.activitydirection
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"auth":"**put_access_token_here**"}' \
         https://**put_your_bitrix24_address**/rest/crm.enum.activitydirection
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.enum.activitydirection',
        []
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
     "ID": 0,
     "NAME": "",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    },
    {
     "ID": 1,
     "NAME": "Incoming",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    },
    {
     "ID": 2,
     "NAME": "Outgoing",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    }
],
"time": {
    "start": 1750149628.891727,
    "finish": 1750149628.919669,
    "duration": 0.02794194221496582,
    "processing": 0.00030112266540527344,
    "date_start": "2025-06-17T11:40:28+02:00",
    "date_finish": "2025-06-17T11:40:28+02:00",
    "operating_reset_at": 1750150228,
    "operating": 0
}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../../data-types.md) | Array with activity directions [(detailed description)](#result) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

#### Fields of the result Array {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../../data-types.md) | Identifier of the activity direction ||
|| **NAME**
[`string`](../../../../data-types.md) | Name of the activity direction ||
|| **SYMBOL_CODE**
[`string`](../../../../data-types.md) | Symbolic code ||
|| **SYMBOL_CODE_SHORT**
[`string`](../../../../data-types.md) | Short symbolic code ||
|#

## Error Handling

The method does not return errors.

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./../index.md)