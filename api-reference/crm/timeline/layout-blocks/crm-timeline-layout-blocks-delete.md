# Delete a set of additional content blocks for the timeline record crm.timeline.layout.blocks.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `crm.timeline.layout.blocks.delete` removes a set of additional content blocks for a timeline record.

Within the application, only the set of additional content blocks that was installed through this application can be deleted.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId*** 
[`integer`](../../../data-types.md) | Identifier of the entity type to which the timeline record is linked ||
|| **entityId*** 
[`integer`](../../../data-types.md) | Identifier of the entity to which the timeline record is linked ||
|| **timelineId*** 
[`integer`](../../../data-types.md) | Identifier of the timeline record ||
|#

## Code Examples

Delete a set of additional content blocks for the timeline record with `id = 8`, linked to the deal with `id = 4`:

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"entityId":4,"timelineId":8}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.layout.blocks.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"entityId":4,"timelineId":8,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.layout.blocks.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.timeline.layout.blocks.delete',
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
        'crm.timeline.layout.blocks.delete',
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

Returns `{ success: true }` in case of successful deletion of the set of additional content blocks, otherwise `null`.

```json
{
    "success": true
}
```

## Error Handling

HTTP status: **400**

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
|| `OWNER_NOT_FOUND` | The element to which the timeline record is linked was not found ||
|| `NOT_FOUND` | The timeline record or the set of additional content blocks was not found ||
|| `ACCESS_DENIED` | Access denied ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-timeline-layout-blocks-set.md)
- [{#T}](./crm-timeline-layout-blocks-get.md)
- [{#T}](./content-blocks-test-app.md)