# Get Field Descriptions for Custom Field Type "Enumeration" (List) crm.userfield.enumeration.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.userfield.enumeration.fields` returns field descriptions for a custom field of type "enumeration" (list).

## Method Parameters

No parameters.

## Code Examples

{% include [Example Notes](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.userfield.enumeration.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.userfield.enumeration.fields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.userfield.enumeration.fields",
    		{}
    	);

    	const result = response.getData().result;
    	console.dir(result);
    }
    catch(error)
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
                'crm.userfield.enumeration.fields',
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
        echo 'Error fetching CRM userfield enumeration fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.userfield.enumeration.fields",
        {},
        function(result)
        {
            if(result.error())
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
        'crm.userfield.enumeration.fields',
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
    "result": {
        "ID": {
            "type": "int",
            "title": "ID",
            "isReadOnly": true
        },
        "SORT": {
            "type": "int",
            "title": "Sorting"
        },
        "VALUE": {
            "type": "string",
            "title": "Value"
        },
        "DEF": {
            "type": "string",
            "title": "Default Value"
        },
        "DEL": {
            "type": "string",
            "title": "Element Deletion Flag"
        }
    },
    "time": {
        "start": 1773992153,
        "finish": 1773992153.048253,
        "duration": 0.04825305938720703,
        "processing": 0,
        "date_start": "2026-03-20T10:35:53+02:00",
        "date_finish": "2026-03-20T10:35:53+02:00",
        "operating_reset_at": 1773992753,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with field descriptions [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Fields of the result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the list value ||
|| **SORT**
[`integer`](../../data-types.md) | Sorting order ||
|| **VALUE**
[`string`](../../data-types.md) | Value of the list item ||
|| **DEF**
[`string`](../../data-types.md) | Default value indicator ||
|| **DEL**
[`string`](../../data-types.md) | Element deletion flag ||
|#

## Error Handling

{% include notitle [error handling](../../../../_includes/error-info.md) %}

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-userfield-fields.md)
- [{#T}](./crm-userfield-types.md)
- [{#T}](./crm-userfield-settings-fields.md)