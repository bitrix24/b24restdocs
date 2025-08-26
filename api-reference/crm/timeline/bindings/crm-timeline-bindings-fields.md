# Get CRM entity bindings fields and timeline record in crm.timeline.bindings.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

This method retrieves a list of available fields for linking CRM entities and timeline records.

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
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.bindings.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.bindings.fields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod("crm.timeline.bindings.fields");
    	
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
                'crm.timeline.bindings.fields'
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching timeline bindings fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.timeline.bindings.fields",
        result => {
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
        'crm.timeline.bindings.fields'
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
        "OWNER_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Timeline record ID"
        },
        "ENTITY_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Entity ID"
        },
        "ENTITY_TYPE": {
            "type": "string",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Entity type"
        }
    },
    "time": {
        "start": 1715091541.642592,
        "finish": 1715091541.730599,
        "duration": 0.08800697326660156,
        "date_start": "2024-05-03T17:19:01+02:00",
        "date_finish": "2024-05-03T17:19:01+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response. Contains [fields](#fields) linking timeline records to CRM entities ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

#### List of Fields {#fields}

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **OWNER_ID***
[`integer`](../../../data-types.md) | Identifier of the timeline record. Read-only ||
|| **ENTITY_ID***
[`integer`](../../../data-types.md) | `ID` of the CRM entity to which the comment is linked. Immutable ||
|| **ENTITY_TYPE***
[`string`](../../../data-types.md) | Type of the entity to which the comment is linked. Immutable. Possible values:
- `lead` — lead
- `deal` — deal
- `contact` — contact
- `company` — company
- `order` — order
  ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":0,
    "error_description":"error"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-timeline-bindings-bind.md)
- [{#T}](./crm-timeline-bindings-list.md)
- [{#T}](./crm-timeline-bindings-unbind.md)