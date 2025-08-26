# Get directory items by type crm.status.entity.items

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.status.entity.items` returns all directory items by the identifier `ENTITY_ID`, sorted by the `SORT` field. This method is similar to [crm.status.list](crm-status-list.md), except that the latter allows you to define sorting rules.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityId*** 
[`string`](../../data-types.md) | Type of the directory, for example `DEAL_STAGE`, `SOURCE`. You can get a list of types using the method [crm.status.entity.types](./crm-status-entity-types.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"entityId":"DEAL_STAGE"}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.status.entity.items
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityId":"DEAL_STAGE","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.status.entity.items
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.status.entity.items",
    		{
    			entityId: "DEAL_STAGE"
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
                'crm.status.entity.items',
                [
                    'entityId' => 'DEAL_STAGE'
                ]
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
        echo 'Error fetching status entity items: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.status.entity.items",
        {
            entityId: "DEAL_STAGE"
        },
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
        'crm.status.entity.items',
        [
            'entityId' => 'DEAL_STAGE'
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
    "result": [
        {
            "NAME": "New",
            "SORT": 10,
            "STATUS_ID": "NEW"
        },
        {
            "NAME": "Document Preparation",
            "SORT": 20,
            "STATUS_ID": "PREPARATION"
        },
        {
            "NAME": "Prepayment Invoice",
            "SORT": 30,
            "STATUS_ID": "PREPAYMENT_INVOICE"
        },
        {
            "NAME": "In Progress",
            "SORT": 40,
            "STATUS_ID": "EXECUTING"
        },
        {
            "NAME": "Final Invoice",
            "SORT": 50,
            "STATUS_ID": "FINAL_INVOICE"
        },
        {
            "NAME": "Deal Won",
            "SORT": 60,
            "STATUS_ID": "WON"
        },
        {
            "NAME": "Deal Lost",
            "SORT": 70,
            "STATUS_ID": "LOSE"
        },
        {
            "NAME": "Analyzing Cause of Loss",
            "SORT": 80,
            "STATUS_ID": "APOLOGY"
        }
    ],
    "time": {
        "start": 1752144806.703358,
        "finish": 1752144806.76889,
        "duration": 0.06553196907043457,
        "processing": 0.010729789733886719,
        "date_start": "2025-07-10T13:53:26+02:00",
        "date_finish": "2025-07-10T13:53:26+02:00",
        "operating_reset_at": 1752145406,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array of directory items ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "The parameter entityId is not defined or invalid.",
    "error_description": "The identifier of the directory is not specified or is incorrect."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400`     | `Access denied.` | No permission to perform the operation ||
|| `400`     | `The parameter entityId is not defined or invalid.` | The identifier of the directory is not specified or is incorrect ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-status-fields.md)
- [{#T}](./crm-status-list.md)
- [{#T}](./crm-status-add.md)
- [{#T}](./crm-status-update.md)
- [{#T}](./crm-status-delete.md) 