# Unlink Requisite from Object crm.requisite.link.unregister

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method removes the link between requisites and the object.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | Identifier of the object type to which the link belongs.

The following types can be used:
- deal (value `2`)
- old invoice (value `5`)
- estimate (value `7`)
- new invoice (value `31`)
- other dynamic objects (to get possible values, see the method [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md)).

Identifiers of CRM object types can be obtained using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) 
||
|| **entityId***
[`integer`](../../../data-types.md) | Identifier of the object to which the link belongs. 

Identifiers of objects can be obtained using the following methods: [crm.deal.list](../../deals/crm-deal-list.md), [crm.quote.list](../../quote/crm-quote-list.md), [crm.item.list](../../universal/crm-item-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":31,"entityId":315}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.link.unregister
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":31,"entityId":315,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.link.unregister
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.requisite.link.unregister", {
    			entityTypeId: 31,
    			entityId: 315
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.dir(result);
    	}
    }
    catch(error)
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
                'crm.requisite.link.unregister',
                [
                    'entityTypeId' => 31,
                    'entityId'     => 315,
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
        echo 'Error unregistering requisite link: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.link.unregister", {
            entityTypeId: 31,
            entityId: 315
        },
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
        'crm.requisite.link.unregister',
        [
            'entityTypeId' => 31,
            'entityId' => 315
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
        "start": 1718797597.015625,
        "finish": 1718797598.098317,
        "duration": 1.0826919078826904,
        "processing": 0.13887691497802734,
        "date_start": "2024-06-19T13:46:37+02:00",
        "date_finish": "2024-06-19T13:46:38+02:00",
        "operating": 0.1388249397277832
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the link removal:
- `true` — link removed
- `false` — link not removed
||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "entityId is not defined or invalid."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|  
|| **Code** | **Description** ||
|| `entityTypeId is not defined or invalid` | Object type identifier is not set or has an invalid value ||
|| `entityId is not defined or invalid` | Object identifier is not set or has an invalid value ||
|| `Access denied` | Insufficient access permissions to remove the requisite link ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-link-register.md)
- [{#T}](./crm-requisite-link-get.md)
- [{#T}](./crm-requisite-link-list.md)
- [{#T}](./crm-requisite-link-fields.md)