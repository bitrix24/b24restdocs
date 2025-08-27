# Get Description of CRM Operation Modes crm.enum.settings.mode

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.enum.settings.mode` returns a list of CRM operation modes. Use this method to decode the `ID` value of the type returned by the method [crm.settings.mode.get](../../crm-settings-mode-get.md).

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.enum.settings.mode
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"auth":"**put_access_token_here**"}' \
         https://**put_your_bitrix24_address**/rest/crm.enum.settings.mode
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod("crm.enum.settings.mode");
    	
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
                'crm.enum.settings.mode'
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
        echo 'Error calling crm.enum.settings.mode: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod("crm.enum.settings.mode", result => {
        if (result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    });
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.enum.settings.mode',
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
     "ID": 1,
     "NAME": "Classic CRM",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    },
    {
     "ID": 2,
     "NAME": "Simple CRM",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    }
],
"time": {
    "start": 1750153429.516902,
    "finish": 1750153429.650327,
    "duration": 0.13342499732971191,
    "processing": 0.0002980232238769531,
    "date_start": "2025-06-17T12:43:49+03:00",
    "date_finish": "2025-06-17T12:43:49+03:00",
    "operating_reset_at": 1750154029,
    "operating": 0
}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md) | Array of CRM operation modes [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Fields of the result Array {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the operation mode ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the operation mode ||
|| **SYMBOL_CODE**
[`string`](../../../data-types.md) | Symbolic code ||
|| **SYMBOL_CODE_SHORT**
[`string`](../../../data-types.md) | Short symbolic code ||
|#

## Error Handling

The method does not return errors.

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)