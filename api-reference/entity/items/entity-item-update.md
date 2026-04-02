# Update Entity Storage `entity.item.update`

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access permission level `X` (management) in the data storage

{% note info "" %}

The method works only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %}


## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Identifier of the application's data storage. Use the value specified when creating the storage.

You can obtain the identifier using the [entity.get](../entities/entity-get.md) method. ||
|| **ID**^*^
[`integer`](../../data-types.md) | Identifier of the storage item 

You can obtain the storage item identifier using the [entity.item.get](./entity-item-get.md) method. ||
|| **NAME**
[`string`](../../data-types.md) | New name of the storage item ||
|| **PROPERTY_VALUES**
[`object`](../../data-types.md) | Property values of the item in the format `{"PROPERTY_CODE": value}`.

A list of available property codes can be obtained using the [entity.item.property.get](./properties/entity-item-property-get.md) method.

For file-type properties, use the format described in the article [How to Upload Files](../../files/how-to-upload-files.md). ||
|| **SECTION**
[`integer`](../../data-types.md) | Identifier of the storage section ||
|| **DATE_ACTIVE_FROM**
[`datetime`](../../data-types.md) | Start date of the item's activity ||
|| **DATE_ACTIVE_TO**
[`datetime`](../../data-types.md) | End date of the item's activity ||
|| **PREVIEW_PICTURE**
[`file`](../../data-types.md) | Preview image of the item. File format is described in the article [How to Upload Files](../../files/how-to-upload-files.md).

If `false` is passed, the image will be deleted. ||
|| **DETAIL_PICTURE**
[`file`](../../data-types.md) | Detailed image of the item. File format is described in the article [How to Upload Files](../../files/how-to-upload-files.md).

If `false` is passed, the image will be deleted. ||
|| **UF_**
[`any`](../../data-types.md) | Custom fields of the item `UF_*`.

Passed as separate parameters in the format `"UF_CODE": value`, for example: `"UF_CRM_1_COLOR": "red"`, `"UF_CRM_1_SIZE": 42` ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

Example of updating an item, where:
- `ENTITY` — storage identifier `dish`
- `ID` — item identifier `2333`
- `NAME` — new name
- `PROPERTY_VALUES` — updated property values
- `SECTION` — section identifier

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ENTITY":"dish","ID":2333,"NAME":"Hello, updated world!","PROPERTY_VALUES":{"test":33,"test1":44},"SECTION":219,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/entity.item.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.item.update',
    		{
    			ENTITY: 'dish',
    			ID: 2333,
    			NAME: 'Hello, updated world!',
    			PROPERTY_VALUES: {
    				test: 33,
    				test1: 44,
    			},
    			SECTION: 219,
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
                'entity.item.update',
                [
                    'ENTITY' => 'dish',
                    'ID' => 2333,
                    'NAME' => 'Hello, updated world!',
                    'PROPERTY_VALUES' => [
                        'test' => 33,
                        'test1' => 44,
                    ],
                    'SECTION' => 219,
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
        echo 'Error updating entity item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.item.update',
        {
            ENTITY: 'dish',
            ID: 2333,
            NAME: 'Hello, updated world!',
            PROPERTY_VALUES: {
                test: 33,
                test1: 44,
            },
            SECTION: 219,
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
        'entity.item.update',
        [
            'ENTITY' => 'dish',
            'ID' => 2333,
            'NAME' => 'Hello, updated world!',
            'PROPERTY_VALUES' => [
                'test' => 33,
                'test1' => 44,
            ],
            'SECTION' => 219,
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
        "start": 1774437094,
        "finish": 1774437094.470878,
        "duration": 0.47087788581848145,
        "processing": 0,
        "date_start": "2026-03-25T14:11:34+01:00",
        "date_finish": "2026-03-25T14:11:34+01:00",
        "operating_reset_at": 1774437694,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the item update (`true` — successful) ||
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
|| `ERROR_ARGUMENT` | Argument 'ENTITY' is null or empty | The `ENTITY` parameter was not passed or is empty after cleaning ||
|| `ERROR_ARGUMENT` | Entity code is too long. Max length is N characters. | The `ENTITY` value is too long ||
|| `ERROR_ARGUMENT` | Argument 'ID' is null or empty | The `ID` parameter was not passed or is `<= 0` ||
|| `ERROR_ARGUMENT` | Field validation errors for the item | Invalid input fields were passed ||
|| `ERROR_ENTITY_NOT_FOUND` | Entity not found | Storage with the provided `ENTITY` was not found ||
|| `ERROR_ITEM_NOT_FOUND` | Item not found | Item with the provided `ID` was not found in the storage ||
|| `ACCESS_DENIED` | Access denied! | Insufficient permissions to update the item ||
|| `ACCESS_DENIED` | Access denied! Application context required | No application context (`clientId`) ||
|| `ERROR_CORE` | Internal error updating entity item. Try updating again. | Internal error while updating the item ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./entity-item-add.md)
- [{#T}](./entity-item-get.md)
- [{#T}](./entity-item-delete.md)
- [{#T}](./properties/index.md)