# Delete the entity.section.delete method

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access permission level `W` (write) or `X` (manage) on the data storage

The `entity.section.delete` method removes a section from the application's data storage.

{% note info "" %}

The method works only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %}


## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Identifier of the application's data storage. Use the value specified when creating the storage.

You can obtain the identifier using the [entity.get](../entities/entity-get.md) method. ||
|| **ID**^*^
[`integer`](../../data-types.md) | Identifier of the section to be deleted. ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

Example of deleting a section where:
- `ENTITY` — identifier of the storage `dish`
- `ID` — identifier of the section `673`

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ENTITY":"dish","ID":673,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/entity.section.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.section.delete',
    		{
    			ENTITY: 'dish',
    			ID: 673,
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
                'entity.section.delete',
                [
                    'ENTITY' => 'dish',
                    'ID' => 673,
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
        echo 'Error deleting entity section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.section.delete',
        {
            ENTITY: 'dish',
            ID: 673,
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
        'entity.section.delete',
        [
            'ENTITY' => 'dish',
            'ID' => 673,
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
    "result": true,
    "time": {
        "start": 1774341623,
        "finish": 1774341623.380336,
        "duration": 0.38033604621887207,
        "processing": 0,
        "date_start": "2026-03-24T11:40:23+01:00",
        "date_finish": "2026-03-24T11:40:23+01:00",
        "operating_reset_at": 1774342223,
        "operating": 0.1318378448486328
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | The result of the method execution. Returns `true` for successful deletion. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_SECTION_NOT_FOUND",
    "error_description": "Section not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_ARGUMENT` | Argument 'ENTITY' is null or empty | The `ENTITY` parameter is not provided or is empty after cleaning. ||
|| `ERROR_ARGUMENT` | Entity code is too long. Max length is N characters. | The `ENTITY` value is too long. ||
|| `ERROR_ARGUMENT` | Argument 'ID' is null or empty | The `ID` parameter is not provided or has a value of `0`. ||
|| `ERROR_ENTITY_NOT_FOUND` | Entity not found | The storage with the provided `ENTITY` was not found. ||
|| `ERROR_SECTION_NOT_FOUND` | Section not found | The section with the specified `ID` was not found in this storage. ||
|| `ACCESS_DENIED` | Access denied! | Insufficient permissions to delete the section. ||
|| `ERROR_CORE` | Internal error deleting entity section. Try deleting again. | Internal error while deleting the section. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./entity-section-add.md)
- [{#T}](./entity-section-update.md)
- [{#T}](./entity-section-get.md)
- [{#T}](./index.md)