# Update Sales Funnel crm.category.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with administrative access to the CRM section

This method updates the sales funnel (direction) with the identifier `id`, assigning it new field values from `fields`. If any field is missing in `fields`, its value will remain unchanged.

## Method Parameters

{% include [Footnote about parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`][1] | Identifier of the [system](../../index.md) or [user-defined type](../user-defined-object-types/index.md) of CRM entities for which the funnel will be updated            ||
|| **id***
[`integer`][1] | Identifier of the funnel. It can be obtained using the [`crm.category.list`](./crm-category-list.md) method or when creating a funnel with the [`crm.category.add`](./crm-category-add.md) method ||
|| **fields***
[`object`][1]  |  Field values (detailed description provided [below](#parametr-fields)) for updating the funnel fields in the following structure:

```js
fields: {
    name: "value",
    sort: "value",
    isDefault: "value",
},
```

  ||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`][1] | Name of the funnel. Possible name criteria:
- length cannot exceed `255` characters
- cannot be empty
- cannot consist solely of spaces, tabs, etc.

Defaults to `-` ||
|| **sort**
[`integer`][1] | Sort index. 

Cannot be negative. If a value less than zero is passed to `sort`, it will be ignored and `sort` will be set to `0`.

Defaults to `500` || 
|| **isDefault**
[`boolean`][1] | Indicates whether the funnel is the default funnel. Can have values:
- `Y` — yes, it is
- `N` — no

Defaults to `N`

In deals, the `isDefault` field is not available for modification.

You can check if a field is immutable using the [`crm.category.fields`](./crm-category-fields.md) method. Immutable fields have the property `isReadonly = true` ||
|#


## Code Examples

How to update a funnel with `id = 4` in an SPA with `entityTypeId = 1152`

{% include [Footnote about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1152,"id":4,"fields":{"name":"New Funnel Name","sort":1000,"isDefault":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.category.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1152,"id":4,"fields":{"name":"New Funnel Name","sort":1000,"isDefault":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.category.update
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.category.update",
        {
            entityTypeId: 1152,
            id: 4,
            fields: {
                name: "New Funnel Name",
                sort: 1000,
                isDefault: "Y",
            },
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
        'crm.category.update',
        [
            'entityTypeId' => 1152,
            'id' => 4,
            'fields' => [
                'name' => "New Funnel Name",
                'sort' => 1000,
                'isDefault' => "Y"
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
    "result": {
        "category": {
            "id": 4,
            "name": "New Funnel Name",
            "sort": 1000,
            "entityTypeId": 1152,
            "isDefault": "Y"
        }
    },
    "time": {
        "start": 1718359296.368324,
        "finish": 1718359296.65352,
        "duration": 0.28519606590270996,
        "processing": 0.03645014762878418,
        "date_start": "2024-06-14T12:01:36+02:00",
        "date_finish": "2024-06-14T12:01:36+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response. Contains the [`category`](./crm-category-add.md#category) object ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "SPA not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `NOT_FOUND` | SPA not found | Occurs with incorrect values for `entityTypeId` ||
|| `ENTITY_TYPE_NOT_SUPPORTED` | Entity type `{entityTypeName}` is not supported | Occurs if the CRM object does not support funnels ||
|| `NOT_FOUND` | Element not found | Occurs if the funnel being updated does not exist ||
|| `ACCESS_DENIED` | Access denied | Occurs if the user updating the funnel does not have sufficient permissions ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-category-add.md)
- [{#T}](./crm-category-get.md)
- [{#T}](./crm-category-list.md)
- [{#T}](./crm-category-delete.md)
- [{#T}](./crm-category-fields.md)

[1]: ../../../data-types.md