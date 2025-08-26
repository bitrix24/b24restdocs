# Get Description of CRM Status Fields

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.status.fields` returns the description of CRM reference fields.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.status.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.status.fields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.status.fields",
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
                'crm.status.fields',
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
        echo 'Error fetching CRM status fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.status.fields",
        {},
        function(result) {
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
        'crm.status.fields',
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
            "type": "integer",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "ID"
        },
        "ENTITY_ID": {
            "type": "string",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Entity ID"
        },
        "STATUS_ID": {
            "type": "string",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Status"
        },
        "SORT": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Sorting"
        },
        "NAME": {
            "type": "string",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Name"
        },
        "NAME_INIT": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Default Name"
        },
        "SYSTEM": {
            "type": "char",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "System"
        },
        "CATEGORY_ID": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "CATEGORY_ID"
        },
        "COLOR": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "COLOR"
        },
        "SEMANTICS": {
            "type": "char",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "SEMANTICS"
        },
        "EXTRA": {
            "type": "crm_status_extra",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Additional Fields"
        }
    },
    "time": {
        "start": 1752132864.335154,
        "finish": 1752132864.366912,
        "duration": 0.03175783157348633,
        "processing": 0.002053976058959961,
        "date_start": "2025-07-10T10:34:24+02:00",
        "date_finish": "2025-07-10T10:34:24+02:00",
        "operating_reset_at": 1752133464,
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

#### Fields of the result object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Status identifier, used in methods [crm.status.*](./index.md) ||
|| **ENTITY_ID**
[`string`](../../data-types.md) | Status type identifier ||
|| **STATUS_ID**
[`string`](../../data-types.md) | Status value code, used in CRM object methods ||
|| **SORT**
[`integer`](../../data-types.md) | Sorting order ||
|| **NAME**
[`string`](../../data-types.md) | Name ||
|| **NAME_INIT**
[`string`](../../data-types.md) | Default name ||
|| **SYSTEM**
[`char`](../../data-types.md) | Flag indicating whether the status is system ||
|| **CATEGORY_ID**
[`integer`](../../data-types.md) | Identifier of the funnel to which the status stage belongs ||
|| **COLOR**
[`string`](../../data-types.md) | Color of the status stage for Kanban ||
|| **SEMANTICS**
[`char`](../../data-types.md) | Group of stages:
- `"S"` — success, 
- `"F"` — failure, 
- `""` — in progress ||
|| **EXTRA**
[`object`](../../data-types.md) | Additional fields ||
|#

## Error Handling

The method does not return errors.

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-status-list.md)
- [{#T}](./crm-status-get.md)
- [{#T}](./crm-status-add.md)
- [{#T}](./crm-status-update.md)
- [{#T}](./crm-status-delete.md)