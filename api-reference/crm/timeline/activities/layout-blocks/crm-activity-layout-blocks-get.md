# Get a Set of Additional Content Blocks in a CRM Activity

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: REST Application

This method allows a REST application to retrieve a set of additional content blocks that it has established in the deal.

The REST application can only obtain the set of additional content blocks that it has set up itself.

## Method Parameters

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../../../data-types.md) | Identifier of the CRM object type associated with the deal ||
|| **entityId***
[`integer`](../../../../data-types.md) | Identifier of the CRM object associated with the deal ||
|| **activityId***
[`integer`](../../../../data-types.md) | Identifier of the deal ||
|#

## Code Examples

Retrieve a set of additional content blocks in the deal with `id = 8`, linked to the deal with `id = 4`:

{% include [Note on Examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"entityId":4,"activityId":8}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.layout.blocks.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"entityId":4,"activityId":8,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.layout.blocks.get
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.activity.layout.blocks.get',
        {
            entityTypeId: 2,
            entityId: 4,
            activityId: 8,
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
        'crm.activity.layout.blocks.get',
        [
            'entityTypeId' => 2,
            'entityId' => 4,
            'activityId' => 8
        ]
    );
    echo '';
    print_r($result);
    echo '';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

Returns an `object` with the key `layout`, containing [RestAppLayoutDto](../structure/rest-app-layout-dto.md).

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
                    "text": "Open Deal",
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

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ERROR_WRONG_CONTEXT` | The method can only be called in the context of a REST application ||
|| `OWNER_NOT_FOUND` | The entity associated with the deal was not found ||
|| `NOT_FOUND` | The deal was not found ||
|| `ACCESS_DENIED` | Access is denied ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-activity-layout-blocks-set.md)
- [{#T}](./crm-activity-layout-blocks-delete.md)