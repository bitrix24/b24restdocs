# Get Properties of Storage Elements `entity.item.property.get`

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`entity`](../../../scopes/permissions.md)
>
> Who can execute the method: a user authorized in the application

The method `entity.item.property.get` returns the properties of the application's data storage elements.

{% note info "" %}

The method works only in the context of the [application](../../../../settings/app-installation/index.md).

{% endnote %}


## Method Parameters

{% include [Footnote about parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY**^*^
[`string`](../../../data-types.md) | Identifier of the application's data storage. Use the value specified when creating the storage.

You can obtain the identifier using the [entity.get](../../entities/entity-get.md) method. ||
|| **PROPERTY**
[`string`](../../../data-types.md) | Property code.

If the parameter is not provided, the method returns a list of all properties of the storage.

Allowed characters are `a-z`, `A-Z`, `0-9`, `_` ||
|#

## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

Example of retrieving a list of properties for elements, where `ENTITY` is the identifier of the storage `dish`.

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ENTITY":"dish","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/entity.item.property.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.item.property.get',
    		{
    			ENTITY: 'dish',
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
                'entity.item.property.get',
                [
                    'ENTITY' => 'dish',
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
        echo 'Error getting entity item properties: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.item.property.get',
        {
            ENTITY: 'dish',
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
        'entity.item.property.get',
        [
            'ENTITY' => 'dish',
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
    "result": [
        {
            "PROPERTY": "test",
            "NAME": "Test Property",
            "TYPE": "S",
            "SORT": "100"
        },
        {
            "PROPERTY": "test1",
            "NAME": "Second Property",
            "TYPE": "N",
            "SORT": "200"
        }
    ],
    "time": {
        "start": 1774439396,
        "finish": 1774439396.082458,
        "duration": 0.0824580192565918,
        "processing": 0,
        "date_start": "2026-03-25T14:49:56+02:00",
        "date_finish": "2026-03-25T14:49:56+02:00",
        "operating_reset_at": 1774439996,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`result`](#result) | Root element of the response. Contains the property object or an array of properties ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Type result {#result}

#|
|| **Name**
`type` | **Description** ||
|| `object`
[`property`](#property) | Returned if the `PROPERTY` parameter is provided ||
|| [`property[]`](#property) | Returned if the `PROPERTY` parameter is not provided ||
|#

#### Type property {#property}

#|
|| **Name**
`type` | **Description** ||
|| **PROPERTY**
[`string`](../../../data-types.md) | Property code ||
|| **NAME**
[`string`](../../../data-types.md) | Property name ||
|| **TYPE**
[`string`](../../../data-types.md) | Property type (`S`, `N`, `F`) ||
|| **SORT**
[`integer`](../../../data-types.md) | Property sort index ||
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
|| `ERROR_ARGUMENT` | Argument 'ENTITY' is null or empty | The `ENTITY` parameter is not provided or is empty after cleaning ||
|| `ERROR_ARGUMENT` | Entity code is too long. Max length is N characters. | The `ENTITY` value is too long ||
|| `ERROR_ENTITY_NOT_FOUND` | Entity not found | Storage with the provided `ENTITY` not found ||
|| `ERROR_PROPERTY_NOT_FOUND` | Property not found | Property with the provided `PROPERTY` not found ||
|| `ACCESS_DENIED` | Access denied! Application context required | No application context (`clientId`) ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./entity-item-property-add.md)
- [{#T}](./entity-item-property-update.md)
- [{#T}](./entity-item-property-delete.md)
- [{#T}](./index.md)