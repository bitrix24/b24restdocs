# Get a set of additional content blocks for the timeline record crm.timeline.layout.blocks.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `crm.timeline.layout.blocks.get` retrieves a set of additional content blocks for a timeline record.

Within the application, you can only obtain the set of additional content blocks that has been installed through this application.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | Identifier of the CRM object type to which the timeline record is linked ||
|| **entityId***
[`integer`](../../../data-types.md) | Identifier of the CRM object to which the timeline record is linked ||
|| **timelineId***
[`integer`](../../../data-types.md) | Identifier of the timeline record ||
|#

## Code Examples

Retrieve a set of additional content blocks for the timeline record with `id = 8`, linked to the deal with `id = 4`:

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"entityId":4,"timelineId":8}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.layout.blocks.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"entityId":4,"timelineId":8,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.layout.blocks.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.timeline.layout.blocks.get',
    		{
    			entityTypeId: 2, // Deal
    			entityId: 4,     // Deal ID
    			timelineId: 8,   // ID of the timeline record linked to this deal
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
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
                'crm.timeline.layout.blocks.get',
                [
                    'entityTypeId' => 2, // Deal
                    'entityId'     => 4, // Deal ID
                    'timelineId'   => 8, // ID of the timeline record linked to this deal
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
        echo 'Error getting timeline layout blocks: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.timeline.layout.blocks.get',
        {
            entityTypeId: 2, // Deal
            entityId: 4,     // Deal ID
            timelineId: 8,   // ID of the timeline record linked to this deal
        },
        (result) => {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');
    $result = CRest::call(
        'crm.timeline.layout.blocks.get',
        [
            'entityTypeId' => 2,
            'entityId' => 4,
            'timelineId' => 8,
        ]
    );
    echo '';
    print_r($result);
    echo '';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

Returns an `object` with the key `layout`, containing [RestAppLayoutDto](../activities/configurable/structure/rest-app-layout-dto.md).

```json
{
    "layout": {
        "blocks": {
            "block_1": {
                "type": "text",
                "properties": {
                    "value": "Hello!\nWe are starting.",
                    "multiline": true,
                    "bold": true,
                    "color": "base_90"
                }
            },
            "block_2": {
                "type": "largeText",
                "properties": {
                    "value": "Hello!\nWe are starting.\nWe are continuing.\nWe are still working on this.\nWe are continuing.\nWe are close to the result.\nGoodbye."
                }
            },
            "block_3": {
                "type": "link",
                "properties": {
                    "text": "Open deal",
                    "bold": true,
                    "action": {
                        "type": "redirect",
                        "uri": "/crm/deal/details/123/"
                    }
                }
            },
            "block_4": {
                "type": "withTitle",
                "properties": {
                    "title": "Title",
                    "block": {
                        "type": "text",
                        "properties": {
                            "value": "Some value"
                        }
                    }
                }
            }
        }
    }
}
```

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_WRONG_CONTEXT",
    "error_description": "The method can only be called in the context of a rest application"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ERROR_WRONG_CONTEXT` | The method can only be called in the context of a rest application ||
|| `OWNER_NOT_FOUND` | The element to which the timeline record is linked was not found ||
|| `NOT_FOUND` | The timeline record was not found ||
|| `ACCESS_DENIED` | Access denied ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-timeline-layout-blocks-set.md)
- [{#T}](./crm-timeline-layout-blocks-delete.md)
- [{#T}](./content-blocks-test-app.md)