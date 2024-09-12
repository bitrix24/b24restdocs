# Set a Set of Additional Content Blocks in CRM Activity Layout

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: REST Application

This method allows REST applications to set a set of additional content blocks in a CRM activity.

Setting a new set of additional content blocks in an activity will overwrite any previously added set within the same application.

The set of additional content blocks cannot be applied to:
- Configurable activity
- An activity whose type is deprecated

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../../../data-types.md) | Identifier of the CRM entity type to which the activity is linked ||
|| **entityId***
[`integer`](../../../../data-types.md) | Identifier of the CRM entity to which the activity is linked ||
|| **activityId***
[`integer`](../../../../data-types.md) | Identifier of the activity ||
|| **layout***
[`RestAppLayoutDto`](../structure/rest-app-layout-dto.md) | Object describing the set of additional content blocks ||
|#

## Code Examples

For the activity with `id = 8`, linked to the deal with `id = 4`, we will set the following set of additional content blocks:

1. Text
2. Long multiline text
3. Link
4. Block with a title

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"entityId":4,"activityId":8,"layout":{"blocks":{"block_1":{"type":"text","properties":{"value":"Hello!\nWe are starting.","multiline":true,"bold":true,"color":"base_90"}},"block_2":{"type":"largeText","properties":{"value":"Hello!\nWe are starting.\nWe are continuing.\nWe are still working on this.\nWe are continuing.\nWe are close to a result.\nGoodbye."}},"block_3":{"type":"link","properties":{"text":"Open deal","bold":true,"action":{"type":"redirect","uri":"/crm/deal/details/123/"}}},"block_4":{"type":"withTitle","properties":{"title":"Title","block":{"type":"text","properties":{"value":"Some value"}}}}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.layout.blocks.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"entityId":4,"activityId":8,"layout":{"blocks":{"block_1":{"type":"text","properties":{"value":"Hello!\nWe are starting.","multiline":true,"bold":true,"color":"base_90"}},"block_2":{"type":"largeText","properties":{"value":"Hello!\nWe are starting.\nWe are continuing.\nWe are still working on this.\nWe are continuing.\nWe are close to a result.\nGoodbye."}},"block_3":{"type":"link","properties":{"text":"Open deal","bold":true,"action":{"type":"redirect","uri":"/crm/deal/details/123/"}}},"block_4":{"type":"withTitle","properties":{"title":"Title","block":{"type":"text","properties":{"value":"Some value"}}}}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.layout.blocks.set
    ```

- JS

    ```js
    const layout = {
        blocks: {
            'block_1': {
                type: "text",
                properties: {
                    value: "Hello!\nWe are starting.",
                    multiline: true,
                    bold: true,
                    color: "base_90"
                }
            },
            'block_2': {
                type: "largeText",
                properties: {
                    value: "Hello!\nWe are starting.\nWe are continuing.\nWe are still working on this.\nWe are continuing.\nWe are close to a result.\nGoodbye."
                }
            },
            'block_3': {
                type: "link",
                properties: {
                    text: "Open deal",
                    bold: true,
                    action: {
                        type: "redirect",
                        uri: "/crm/deal/details/123/"
                    }
                }
            },
            'block_4': {
                type: "withTitle",
                properties: {
                    title: "Title",
                    block: {
                        type: "text",
                        properties: {
                            value: "Some value"
                        }
                    }
                }
            }
        }
    };
    BX24.callMethod(
        'crm.activity.layout.blocks.set',
        {
            entityTypeId: 2, // Deal
            entityId: 4,     // Deal ID
            activityId: 8,   // ID of the activity linked to this deal
            layout: layout,  // Object describing the set of additional content blocks
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
        'crm.activity.layout.blocks.set',
        [
            'entityTypeId' => 2,
            'entityId' => 4,
            'activityId' => 8,
            'layout' => [
                'blocks' => [
                    'block_1' => [
                        'type' => "text",
                        'properties' => [
                            'value' => "Hello!\nWe are starting.",
                            'multiline' => true,
                            'bold' => true,
                            'color' => "base_90"
                        ]
                    ],
                    'block_2' => [
                        'type' => "largeText",
                        'properties' => [
                            'value' => "Hello!\nWe are starting.\nWe are continuing.\nWe are still working on this.\nWe are continuing.\nWe are close to a result.\nGoodbye."
                        ]
                    ],
                    'block_3' => [
                        'type' => "link",
                        'properties' => [
                            'text' => "Open deal",
                            'bold' => true,
                            'action' => [
                                'type' => "redirect",
                                'uri' => "/crm/deal/details/123/"
                            ]
                        ]
                    ],
                    'block_4' => [
                        'type' => "withTitle",
                        'properties' => [
                            'title' => "Title",
                            'block' => [
                                'type' => "text",
                                'properties' => [
                                    'value' => "Some value"
                                ]
                            ]
                        ]
                    ]
                ]
            ]
        ]
    );
    echo '';
    print_r($result);
    echo '';
    ```

{% endlist %}

## Appearance

If the activity contains more than one set of additional content blocks, they will be displayed in the order they were added.

In the HTML markup, it is explicitly indicated through data attributes which REST application added the set of additional content blocks:
- `data-app-name`: name of the REST application
- `data-rest-client-id`: identifier of the REST application

## Response Handling

HTTP Status: **200**

Returns `{ success: true }` in case of successful writing of the set of additional content blocks, otherwise `null`.

```json
{
    "success": true
}
```

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_WRONG_CONTEXT",
    "error_description": "Method call is only possible in the context of a REST application"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ERROR_WRONG_CONTEXT` | Method call is only possible in the context of a REST application ||
|| `OWNER_NOT_FOUND` | The entity to which the activity is linked was not found ||
|| `NOT_FOUND` | Activity not found ||
|| `ACCESS_DENIED` | Access denied ||
|| `UNSUITABLE_ACTIVITY_TYPE_ERROR` | The type of this activity is not suitable for adding a set of additional content blocks ||
|| `FIELD_IS_REQUIRED` | The `blocks` field in `RestAppLayoutDto` must be filled. ||
|#

The method also returns errors related to the incorrect structure of the set of content blocks. Details can be found in the error message.

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-activity-layout-blocks-get.md)
- [{#T}](./crm-activity-layout-blocks-delete.md)