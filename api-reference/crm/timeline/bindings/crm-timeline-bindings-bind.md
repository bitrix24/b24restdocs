# Add Timeline Record Binding to CRM Entity crm.timeline.bindings.bind

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

This method adds a binding of a timeline record to a CRM entity.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Field values (detailed description provided [below](#parametr-fields)) for adding a binding of a timeline record to a CRM entity in the form of a structure:

```js
fields: {
    "OWNER_ID": "value",
    "ENTITY_ID": "value",
    "ENTITY_TYPE": "value",
},
```

 ||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **OWNER_ID***
[`integer`](../../../data-types.md) | Identifier of the timeline record  ||
|| **ENTITY_ID***
[`integer`](../../../data-types.md) | Identifier `ID` of the CRM entity to which the comment is linked  ||
|| **ENTITY_TYPE***
[`string`](../../../data-types.md) | Type of the entity to which the comment is linked. Possible values: 
- `lead` — lead
- `deal` — deal
- `contact` — contact
- `company` — company
- `order` — order  
 ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"OWNER_ID":1110,"ENTITY_ID":10,"ENTITY_TYPE":"deal"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.bindings.bind
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"OWNER_ID":1110,"ENTITY_ID":10,"ENTITY_TYPE":"deal"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.bindings.bind
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.timeline.bindings.bind",
    		{
    			fields: {
    				"OWNER_ID": 1110,
    				"ENTITY_ID": 10,
    				"ENTITY_TYPE": "deal",
    			},
    		}
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
                'crm.timeline.bindings.bind',
                [
                    'fields' => [
                        'OWNER_ID'    => 1110,
                        'ENTITY_ID'   => 10,
                        'ENTITY_TYPE' => 'deal',
                    ],
                ]
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
        echo 'Error binding timeline: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.timeline.bindings.bind",
        {
            fields: {
                "OWNER_ID": 1110,
                "ENTITY_ID": 10,
                "ENTITY_TYPE": "deal",
            },
        }, result => {
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
        'crm.timeline.bindings.bind',
        [
            'fields' => [
                'OWNER_ID' => 1110,
                'ENTITY_ID' => 10,
                'ENTITY_TYPE' => 'deal',
            ]
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
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the operation. Returns `true` if the binding was successfully created, otherwise — `false` ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "OWNER_ID is not defined or invalid."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | OWNER_ID is not defined or invalid | The required parameter `OWNER_ID` was not provided or the provided `OWNER_ID` is invalid ||
|| Empty string | ENTITY_ID is not defined or invalid. | The required parameter `ENTITY_ID` was not provided or the provided `ENTITY_ID` is invalid ||
|| Empty string | ENTITY_TYPE is not defined or invalid. | The required parameter `ENTITY_TYPE` was not provided or the provided `ENTITY_TYPE` is invalid ||
|| Empty string | Access denied. | No permission to edit the entity in CRM ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-timeline-bindings-list.md)
- [{#T}](./crm-timeline-bindings-unbind.md)
- [{#T}](./crm-timeline-bindings-fields.md)