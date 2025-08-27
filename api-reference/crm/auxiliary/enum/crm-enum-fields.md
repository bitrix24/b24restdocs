# Get Fields of CRM Enumeration Elements `crm.enum.fields`

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.enum.fields` returns information about the fields of enumeration elements.

## Method Parameters

No parameters.

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.enum.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"auth":"**put_access_token_here**"}' \
         https://**put_your_bitrix24_address**/rest/crm.enum.fields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.enum.fields",
    		{}
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.dir(result);
    	}
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
                'crm.enum.fields',
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
        echo 'Error calling crm.enum.fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.enum.fields",
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
        'crm.enum.fields',
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
"result": {
    "ID": {
        "type": "int",
        "isRequired": false,
        "isReadOnly": true,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "title": "ID"
    },
    "NAME": {
        "type": "string",
        "isRequired": false,
        "isReadOnly": true,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "title": "Name"
    },
    "SYMBOL_CODE": {
        "type": "string",
        "isRequired": false,
        "isReadOnly": true,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "title": "Symbol Code"
    },
    "SYMBOL_CODE_SHORT": {
        "type": "string",
        "isRequired": false,
        "isReadOnly": true,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "title": "Short Symbol Code"
    }
},
"time": {
    "start": 1750152521.485259,
    "finish": 1750152521.526358,
    "duration": 0.041098833084106445,
    "processing": 0.00034499168395996094,
    "date_start": "2025-06-17T12:28:41+02:00",
    "date_finish": "2025-06-17T12:28:41+02:00",
    "operating_reset_at": 1750153121,
    "operating": 0
}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with field descriptions [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Fields of the result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`object`](../../../data-types.md) | Identifier ||
|| **NAME**
[`object`](../../../data-types.md) | Name ||
|| **SYMBOL_CODE**
[`object`](../../../data-types.md) | Symbol Code ||
|| **SYMBOL_CODE_SHORT**
[`object`](../../../data-types.md) | Short Symbol Code ||
|#

#### Description of Field Characteristics

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../../data-types.md) | Data type of the field ||
|| **isRequired**
[`boolean`](../../../data-types.md) | Required ||
|| **isReadOnly**
[`boolean`](../../../data-types.md) | Read-only ||
|| **isImmutable**
[`boolean`](../../../data-types.md) | Immutable ||
|| **isMultiple**
[`boolean`](../../../data-types.md) | Multiple ||
|| **isDynamic**
[`boolean`](../../../data-types.md) | Dynamic ||
|| **title**
[`string`](../../../data-types.md) | Field name ||
|#

## Error Handling

The method does not return errors.

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)