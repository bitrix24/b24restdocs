# Delete Entity Storage Method: entity.item.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access permission level `X` (management) on the data storage

The `entity.item.delete` method removes an item from the application's data storage.

{% note info "" %}

This method works only within the context of an [application](../../../settings/app-installation/index.md).

{% endnote %}


## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Identifier of the application's data storage. Use the value specified when creating the storage.

You can obtain the identifier using the [entity.get](../entities/entity-get.md) method ||
|| **ID**^*^
[`integer`](../../data-types.md) | Identifier of the storage item to be deleted.

You can obtain the storage item identifier using the [entity.item.get](./entity-item-get.md) method ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

Example of deleting an item, where:
- `ENTITY` — storage identifier `dish`
- `ID` — item identifier `2333`

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ENTITY":"dish","ID":2333,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/entity.item.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.item.delete',
    		{
    			ENTITY: 'dish',
    			ID: 2333,
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
                'entity.item.delete',
                [
                    'ENTITY' => 'dish',
                    'ID' => 2333,
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
        echo 'Error deleting entity item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.item.delete',
        {
            ENTITY: 'dish',
            ID: 2333,
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
        'entity.item.delete',
        [
            'ENTITY' => 'dish',
            'ID' => 2333,
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
        "start": 1774437630,
        "finish": 1774437631.335014,
        "duration": 1.3350141048431396,
        "processing": 1,
        "date_start": "2026-03-25T14:20:30+01:00",
        "date_finish": "2026-03-25T14:20:31+01:00",
        "operating_reset_at": 1774438230,
        "operating": 0.5981030464172363
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the item deletion (`true` — successful) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_ITEM_NOT_FOUND",
    "error_description": "Item not found"
}
```

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "Argument 'ENTITY' is null or empty",
    "argument": "ENTITY"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | Argument 'ENTITY' is null or empty | The `ENTITY` parameter was not provided or is empty after cleanup ||
|| `ERROR_ARGUMENT` | Entity code is too long. Max length is N characters. | The `ENTITY` value is too long ||
|| `ERROR_ARGUMENT` | Argument 'ID' is null or empty | The `ID` parameter was not provided or is `<= 0` ||
|| `ERROR_ENTITY_NOT_FOUND` | Entity not found | The storage with the provided `ENTITY` was not found ||
|| `ERROR_ITEM_NOT_FOUND` | Item not found | The item with the provided `ID` was not found in the storage ||
|| `ACCESS_DENIED` | Access denied! | Insufficient permissions to delete the item ||
|| `ACCESS_DENIED` | Access denied! Application context required | No application context (`clientId`) ||
|| `ERROR_CORE` | Internal error deleting entity item. Try deleting again. | Internal error while deleting the item ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./entity-item-add.md)
- [{#T}](./entity-item-update.md)
- [{#T}](./entity-item-get.md)
- [{#T}](./properties/index.md)