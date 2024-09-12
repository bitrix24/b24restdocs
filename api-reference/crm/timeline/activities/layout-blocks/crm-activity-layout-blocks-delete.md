# Delete a set of additional content blocks in the CRM activity layout

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: REST Application

This method allows a REST application to delete a set of additional content blocks that it has previously set for the CRM activity.

A REST application can only delete the set of additional content blocks that it has installed.

## Method Parameters

{% include [Footnote on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../../../data-types.md) | Identifier of the CRM object type to which the activity is linked ||
|| **entityId***
[`integer`](../../../../data-types.md) | Identifier of the CRM object to which the activity is linked ||
|| **activityId***
[`integer`](../../../../data-types.md) | Identifier of the activity ||
|#

## Code Examples

Delete a set of additional content blocks in the activity with `id = 8`, linked to the deal with `id = 4`:

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"entityId":4,"activityId":8}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.layout.blocks.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"entityId":4,"activityId":8,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.layout.blocks.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.activity.layout.blocks.delete',
        {
            entityTypeId: 2, // Deal
            entityId: 4,     // Deal ID
            activityId: 8,   // ID of the activity linked to this deal
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
        'crm.activity.layout.blocks.delete',
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

HTTP status: **200**

Returns `{ success: true }` if the set of additional content blocks was successfully deleted; otherwise, it returns `null`.

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

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ERROR_WRONG_CONTEXT` | The method can only be called in the context of a REST application ||
|| `OWNER_NOT_FOUND` | The entity to which the activity is linked was not found ||
|| `NOT_FOUND` | The activity was not found ||
|| `ACCESS_DENIED` | Access denied ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-activity-layout-blocks-set.md)
- [{#T}](./crm-activity-layout-blocks-get.md)