# Get Sales Funnel by Id crm.category.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access permission to the funnel "Read"

This method retrieves information about the funnel (direction) with the identifier `id`.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`][1] | Identifier of the [system](../../index.md) or [user-defined type](../user-defined-object-types/index.md) of the CRM object for which we want to get the funnel ||
|| **id***
[`integer`][1] | Identifier of the funnel. It can be obtained using the [`crm.category.list`](./crm-category-list.md) method or when creating a funnel with the [`crm.category.add`](./crm-category-add.md) method ||
|#

## Code Examples

How to get information about the funnel with `id` = `1`, located in deals.

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"id":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.category.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.category.get
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.category.get",
        {
            entityTypeId: 2,
            id: 1,
        },
        (result) => 
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.category.get',
        [
            'entityTypeId' => 2,
            'id' => 1
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
    "result": {
        "category": {
            "id": 1,
            "name": "New Funnel #1",
            "sort": 200,
            "entityTypeId": 2,
            "isDefault": "N",
            "originId": "",
            "originatorId": ""
        }
    },
    "time": {
        "start": 1718291429.253404,
        "finish": 1718291429.654617,
        "duration": 0.4012131690979004,
        "processing": 0.025265216827392578,
        "date_start": "2024-06-13T17:10:29+02:00",
        "date_finish": "2024-06-13T17:10:29+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`][1] | Root element of the response. Contains the [`category`](./crm-category-add.md#category) object ||
|| **time**
[`time`][1] | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Element not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `NOT_FOUND` | SPA not found | Occurs when invalid values for `entityTypeId` are provided ||
|| `ENTITY_TYPE_NOT_SUPPORTED` | Entity type `{entityTypeName}` is not supported | Occurs if the CRM object does not support funnels ||
|| `NOT_FOUND` | Element not found | Occurs if no funnels exist with the given parameters ||
|| `ACCESS_DENIED` | Access denied | Occurs if the user does not have permission to view elements of this funnel ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-category-add.md)
- [{#T}](./crm-category-update.md)
- [{#T}](./crm-category-list.md)
- [{#T}](./crm-category-delete.md)
- [{#T}](./crm-category-fields.md)

[1]: ../../../data-types.md