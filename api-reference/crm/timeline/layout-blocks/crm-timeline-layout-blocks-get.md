# Get a set of additional content blocks for the CRM timeline record crm.timeline.layout.blocks.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: REST Application

This method allows a REST application to retrieve the set of additional content blocks that it has established for the timeline record.

A REST application can only obtain the set of additional content blocks that it has set up.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | Identifier of the CRM object type associated with the timeline record ||
|| **entityId***
[`integer`](../../../data-types.md) | Identifier of the CRM object associated with the timeline record ||
|| **timelineId***
[`integer`](../../../data-types.md) | Identifier of the timeline record ||
|#

## Code Examples

Retrieve the set of additional content blocks for the timeline record with `id = 8`, linked to the deal with `id = 4`:

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

- PHP

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

HTTP Status: **200**

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

HTTP Status: **400**

```json
{
    "error": "ERROR_WRONG_CONTEXT",
    "error_description": "The method can only be called in the context of a REST application"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ERROR_WRONG_CONTEXT` | The method can only be called in the context of a REST application ||
|| `OWNER_NOT_FOUND` | The entity to which the timeline record is linked was not found ||
|| `NOT_FOUND` | The timeline record was not found ||
|| `ACCESS_DENIED` | Access denied ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-timeline-layout-blocks-set.md)
- [{#T}](./crm-timeline-layout-blocks-delete.md)
- [{#T}](./content-blocks-test-app.md)