# Update Property of Storage Elements `entity.item.property.update`

> Scope: [`entity`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access permission level `X` (management) in the data storage

The method `entity.item.property.update` modifies the property of elements in the application's data storage.

{% note info "" %}

The method works only in the context of the [application](../../../../settings/app-installation/index.md).

{% endnote %}

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY**^*^
[`string`](../../../data-types.md) | Identifier of the application's data storage. Use the value specified when creating the storage.

You can obtain the identifier using the [entity.get](../../entities/entity-get.md) method. ||
|| **PROPERTY**^*^
[`string`](../../../data-types.md) | Code of the existing property to be modified.

You can obtain the property code using the [entity.item.property.get](./entity-item-property-get.md) method. ||
|| **PROPERTY_NEW**
[`string`](../../../data-types.md) | New property code.

Allowed characters are `a-z`, `A-Z`, `0-9`, `_` ||
|| **NAME**
[`string`](../../../data-types.md) | New property name. ||
|| **TYPE**
[`string`](../../../data-types.md) | New property type:
- `S` — string
- `N` — number
- `F` — file ||
|| **SORT**
[`integer`](../../../data-types.md) | Property sort index. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of updating a property where:
- `ENTITY` — storage identifier `dish`
- `PROPERTY` — original property code `new_prop`
- `PROPERTY_NEW` — new property code `updated_prop`
- `NAME`, `SORT` — new values

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ENTITY":"dish","PROPERTY":"new_prop","PROPERTY_NEW":"updated_prop","NAME":"Updated Property","SORT":200,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/entity.item.property.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.item.property.update',
    		{
    			ENTITY: 'dish',
    			PROPERTY: 'new_prop',
    			PROPERTY_NEW: 'updated_prop',
    			NAME: 'Updated Property',
    			SORT: 200,
    		}
    	);

    	const result = response.getData().result;
    	console.info(result);
    }
    catch (error)
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
                'entity.item.property.update',
                [
                    'ENTITY' => 'dish',
                    'PROPERTY' => 'new_prop',
                    'PROPERTY_NEW' => 'updated_prop',
                    'NAME' => 'Updated Property',
                    'SORT' => 200,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo '<pre>';
        print_r($result);
        echo '</pre>';

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating entity item property: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.item.property.update',
        {
            ENTITY: 'dish',
            PROPERTY: 'new_prop',
            PROPERTY_NEW: 'updated_prop',
            NAME: 'Updated Property',
            SORT: 200,
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'entity.item.property.update',
        [
            'ENTITY' => 'dish',
            'PROPERTY' => 'new_prop',
            'PROPERTY_NEW' => 'updated_prop',
            'NAME' => 'Updated Property',
            'SORT' => 200,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1774442553,
        "finish": 1774442553.149827,
        "duration": 0.1498270034790039,
        "processing": 0,
        "date_start": "2026-03-25T15:42:33+02:00",
        "date_finish": "2026-03-25T15:42:33+02:00",
        "operating_reset_at": 1774443153,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the property update (`true` — successful) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_PROPERTY_NOT_FOUND",
    "error_description": "Property not found"
}
```

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "Argument 'ENTITY' is null or empty",
    "argument": "ENTITY"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | Argument 'ENTITY' is null or empty | The `ENTITY` parameter was not provided or is empty after cleaning. ||
|| `ERROR_ARGUMENT` | Entity code is too long. Max length is N characters. | The value of `ENTITY` is too long. ||
|| `ERROR_ARGUMENT` | Wrong entity item property type | An invalid `TYPE` was provided. ||
|| `ERROR_ARGUMENT` | Cannot change property type to File | Attempting to change the property type to `F`. ||
|| `ERROR_ENTITY_NOT_FOUND` | Entity not found | The storage with the provided `ENTITY` was not found. ||
|| `ERROR_PROPERTY_NOT_FOUND` | Property not found | The property with the provided `PROPERTY` was not found. ||
|| `ERROR_PROPERTY_ALREADY_EXISTS` | Property <PROPERTY_NEW> already exists | A property with the code `PROPERTY_NEW` already exists. ||
|| `ACCESS_DENIED` | Access denied! | Insufficient permissions to modify the property. ||
|| `ACCESS_DENIED` | Access denied! Application context required | No application context (`clientId`). ||
|| `ERROR_CORE` | Internal error updating entity property. Try updating again. | An internal error occurred while updating the property. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./entity-item-property-get.md)
- [{#T}](./entity-item-property-add.md)
- [{#T}](./entity-item-property-delete.md)
- [{#T}](./index.md)