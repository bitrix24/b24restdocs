# Add a New Sales Funnel crm.category.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with administrative access to the CRM section

This method creates a new sales funnel (direction) for the CRM object type with the identifier `entityTypeId`.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId*** 
[`integer`][1] | Identifier of the [system](../../index.md) or [user-defined type](../user-defined-object-types/index.md) of the CRM entity for which the new funnel will be created ||
|| **fields***
[`object`][1]  | Field values (detailed description provided [below](#parametr-fields)) for adding a new funnel in the form of a structure:

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

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`][1] | Name of the funnel. The name can be:
- length cannot exceed `255` characters
- cannot be empty
- cannot consist solely of spaces, tabs, etc.

Defaults to `-` ||
|| **sort**
[`integer`][1] | Sort index. 

Cannot be negative. If a value less than zero is passed to `sort`, it will be ignored and set to `sort = 0`

Defaults to `500` || 
|| **isDefault**
[`boolean`][1] | Indicates whether the funnel is the default funnel. Can have values:
- `Y` — yes, it is
- `N` — no

Defaults to `N`.

In deals, the `isDefault` field is not available for modification. 

Restrictions on changing the `isDefault` field in SPAs:
- the default direction cannot be deleted,
- when creating a new direction and passing it the flag `isDefault: "Y"`, the old default direction will cease to be the default direction,
- when changing the default direction, it cannot be made a non-default direction,
- when changing a non-default direction and passing it the flag `isDefault: "Y"`, the old default direction will cease to be the default direction.

If the display of directions in the interface is disabled for an existing SPA, working with directions via REST is still possible.
||
|#

## Code Examples

Create a new default funnel in the SPA with `entityTypeId = 1152`.

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId": 1152, "fields": {"name": "New Default Funnel", "sort": 50, "isDefault": "Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.category.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId": 1152, "fields": {"name": "New Default Funnel", "sort": 50, "isDefault": "Y"}, "auth": "**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.category.add
    ```

- JS

	```js
    BX24.callMethod(
        "crm.category.add",
        {
            entityTypeId: 1152,
            fields: {
                name: "New Default Funnel",
                sort: 50,
                isDefault: 'Y',
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
        'crm.category.add',
        [
            'entityTypeId' => 1152,
            'fields' => [
                'name' => "New Default Funnel",
                'sort' => 50,
                'isDefault' => 'Y',
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
            "id": 5,
            "name": "New Default Funnel",
            "sort": 50,
            "entityTypeId": 1152,
            "isDefault": "Y"
        }
    },
    "time": {
        "start": 1718116794.208887,
        "finish": 1718116794.666272,
        "duration": 0.4573848247528076,
        "processing": 0.1496260166168213,
        "date_start": "2024-06-11T16:39:54+02:00",
        "date_finish": "2024-06-11T16:39:54+02:00",
        "operating": 0
    }
}
```

### Returned Values
#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`][1] | Root element of the response. Contains the [`category`](#category) object with information about the funnel ||
|| **time**
[`object`][1] | Object containing information about the execution time of the request  ||
|#

#### Object category {#category}

#| 
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`][1] | Identifier of the funnel (direction) ||
|| **name**
[`string`][1] | Name of the funnel ||
|| **sort**
[`integer`][1] | Sort index ||
|| **entityTypeId**
[`integer`][1] | Identifier of the [system](../../index.md) or [user-defined type](../user-defined-object-types/index.md) to which the funnel belongs ||
|| **isDefault**
[`boolean`][1] | Indicates whether the funnel is the default funnel ||
|| **originId**
[`string`][1] | Identifier of the data source.

Exists only in deals ||
|| **originatorId**
[`string`][1] | Identifier of the element in the data source.

Exists only in deals ||
|| **isSystem** 
[`boolean`][1] | Indicates whether the funnel is a system funnel.

Exists only in SPAs ||
|| **code**
[`string`][1] | Alias for system funnels.

Exists only in SPAs ||
|#

## Error Handling

HTTP status: **160**, **400**

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
|| `ADDING_DISABLED` | Adding a system category is prohibited | Occurs when attempting to create a system funnel in SPAs ||
|| `ACCESS_DENIED` | Access denied | Occurs if the user does not have sufficient rights to add a funnel ||
|| `0` | Field 'NAME' is required | Occurs if the required field `name` is not provided ||
|| `0` | Default client category does not support updating default state | Occurs when attempting to create a default funnel for `contacts` or `companies` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-category-update.md)
- [{#T}](./crm-category-get.md)
- [{#T}](./crm-category-list.md)
- [{#T}](./crm-category-delete.md)
- [{#T}](./crm-category-fields.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-category-to-spa.md)

[1]: ../../../data-types.md