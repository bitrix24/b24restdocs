# Add Property to Data Storage Elements `entity.item.property.add`

> Scope: [`entity`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access permission level `X` (management) in the data storage

The method `entity.item.property.add` adds a property to the elements of the application's data storage.

{% note info "" %}

The method works only in the context of the [application](../../../../settings/app-installation/index.md).

{% endnote %}

## Method Parameters

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY**^*^
[`string`](../../../data-types.md) | Identifier of the application's data storage. Use the value specified when creating the storage.

You can obtain the identifier using the [entity.get](../../entities/entity-get.md) method. ||
|| **PROPERTY**^*^
[`string`](../../../data-types.md) | Code of the new property.

Allowed characters are `a-z`, `A-Z`, `0-9`, `_` ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the property ||
|| **TYPE**
[`string`](../../../data-types.md) | Type of the property:
- `S` — string
- `N` — number
- `F` — file ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting index of the property ||
|#

## Code Examples

{% include [Example Note](../../../../_includes/examples.md) %}

Example of adding a property where:
- `ENTITY` — identifier of the storage `dish`
- `PROPERTY` — property code `new_prop`
- `NAME` — name of the property
- `TYPE` — type `S`
- `SORT` — sorting index

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ENTITY":"dish","PROPERTY":"new_prop","NAME":"New Property","TYPE":"S","SORT":100,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/entity.item.property.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.item.property.add',
    		{
    			ENTITY: 'dish',
    			PROPERTY: 'new_prop',
    			NAME: 'New Property',
    			TYPE: 'S',
    			SORT: 100,
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
                'entity.item.property.add',
                [
                    'ENTITY' => 'dish',
                    'PROPERTY' => 'new_prop',
                    'NAME' => 'New Property',
                    'TYPE' => 'S',
                    'SORT' => 100,
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
        echo 'Error adding entity item property: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.item.property.add',
        {
            ENTITY: 'dish',
            PROPERTY: 'new_prop',
            NAME: 'New Property',
            TYPE: 'S',
            SORT: 100,
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
        'entity.item.property.add',
        [
            'ENTITY' => 'dish',
            'PROPERTY' => 'new_prop',
            'NAME' => 'New Property',
            'TYPE' => 'S',
            'SORT' => 100,
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
        "start": 1774440592,
        "finish": 1774440592.563202,
        "duration": 0.563201904296875,
        "processing": 0,
        "date_start": "2026-03-25T15:09:52+01:00",
        "date_finish": "2026-03-25T15:09:52+01:00",
        "operating_reset_at": 1774441192,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of adding the property (`true` — successful) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_PROPERTY_ALREADY_EXISTS",
    "error_description": "Property already exists"
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
|| `ERROR_ARGUMENT` | Argument 'ENTITY' is null or empty | The `ENTITY` parameter is not provided or is empty after cleaning ||
|| `ERROR_ARGUMENT` | Entity code is too long. Max length is N characters. | The `ENTITY` value is too long ||
|| `ERROR_ARGUMENT` | Argument 'PROPERTY' is null or empty | The `PROPERTY` parameter is not provided ||
|| `ERROR_ARGUMENT` | Wrong entity item property type | An invalid `TYPE` was provided ||
|| `ERROR_ENTITY_NOT_FOUND` | Entity not found | Storage with the provided `ENTITY` was not found ||
|| `ERROR_PROPERTY_ALREADY_EXISTS` | Property already exists | A property with the provided `PROPERTY` already exists ||
|| `ACCESS_DENIED` | Access denied! | Insufficient permissions to add the property ||
|| `ACCESS_DENIED` | Access denied! Application context required | No application context (`clientId`) ||
|| `ERROR_UNSUPPORTED_PROPERTY_TYPE` | Invalid property type | Attempted to create a property with type `L` ||
|| `ERROR_CORE` | Internal error adding entity property. Try adding again. | Internal error while adding the property ||
|| `ERROR_CORE` | Property code cannot start with a digit | An invalid property code was provided in `PROPERTY` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./entity-item-property-get.md)
- [{#T}](./entity-item-property-update.md)
- [{#T}](./entity-item-property-delete.md)
- [{#T}](./index.md)