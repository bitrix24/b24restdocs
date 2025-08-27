# Get enumeration items "Activity Notification Type" crm.enum.activitynotifytype

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "Method development has been discontinued" %}

The method `crm.enum.activitynotifytype` continues to function, but it is related to deprecated methods [crm.activity.*](../../../timeline/activities/index.md). A more current alternative is the methods [crm.activity.todo.*](../../../timeline/activities/todo/index.md). 

{% endnote %}

The method `crm.enum.activitynotifytype` returns notification types for the `NOTIFY_TYPE` field of [meetings and calls](../../../timeline/activities/index.md).

## Method Parameters

No parameters.

## Code Examples

{% include [Examples note](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.enum.activitynotifytype
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"auth":"**put_access_token_here**"}' \
         https://**put_your_bitrix24_address**/rest/crm.enum.activitynotifytype
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.enum.activitynotifytype",
    		{}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
    }
    catch( error )
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
                'crm.enum.activitynotifytype',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling crm.enum.activitynotifytype: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.enum.activitynotifytype",
        {},
        function(result) {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.enum.activitynotifytype',
        []
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
"result": [
    {
     "ID": 0,
     "NAME": "",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    },
    {
     "ID": 1,
     "NAME": "min.",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    },
    {
     "ID": 2,
     "NAME": "hr.",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    },
    {
     "ID": 3,
     "NAME": "day.",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    }
],
"time": {
    "start": 1750150460.961181,
    "finish": 1750150461.006235,
    "duration": 0.045053958892822266,
    "processing": 0.00042510032653808594,
    "date_start": "2025-06-17T11:54:20+02:00",
    "date_finish": "2025-06-17T11:54:21+02:00",
    "operating_reset_at": 1750151061,
    "operating": 0
}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../../data-types.md) | Array with notification types for activity start [(detailed description)](#result) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

#### Fields of the result array {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../../data-types.md) | Identifier of the notification type ||
|| **NAME**
[`string`](../../../../data-types.md) | Name of the notification type ||
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