# Delete Data Store entity.delete

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: a user with `X` (management) access permission on the data store

The `entity.delete` method removes the application's data store.

{% note info "" %}

This method only works in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %}


## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Identifier of the application's data store. Use the value specified when creating the store.

You can obtain the identifier using the [entity.get](./entity-get.md) method ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

Example of deleting a store where `ENTITY` is the identifier `dish_v2`.

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ENTITY":"dish_v2","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/entity.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.delete',
    		{
    			ENTITY: 'dish_v2',
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
                'entity.delete',
                [
                    'ENTITY' => 'dish_v2',
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
        echo 'Error deleting entity: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.delete',
        {
            ENTITY: 'dish_v2',
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
        'entity.delete',
        [
            'ENTITY' => 'dish_v2',
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
        "start": 1774271103,
        "finish": 1774271103.40342,
        "duration": 0.40341997146606445,
        "processing": 0,
        "date_start": "2026-03-23T16:05:03+01:00",
        "date_finish": "2026-03-23T16:05:03+01:00",
        "operating_reset_at": 1774271703,
        "operating": 0.13090085983276367
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | The result of the method execution. Returns `true` for successful deletion ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_ENTITY_NOT_FOUND",
    "error_description": "Entity not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | Argument 'ENTITY' is null or empty | The `ENTITY` parameter is not provided or is empty after cleanup ||
|| `ERROR_ARGUMENT` | Entity code is too long. Max length is N characters. | The `ENTITY` value is too long ||
|| `ERROR_ENTITY_NOT_FOUND` | Entity not found | The store with the provided `ENTITY` was not found ||
|| `ACCESS_DENIED` | Access denied! | Insufficient permissions to delete the store ||
|| `ERROR_CORE` | Internal error deleting entity. Try deleting again. | Internal error while deleting the store ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./entity-add.md)
- [{#T}](./entity-update.md)
- [{#T}](./entity-get.md)
- [{#T}](./entity-rights.md)