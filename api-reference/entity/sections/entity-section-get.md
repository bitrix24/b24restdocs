# Get the List of Sections entity.section.get

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user upon application authorization

The method `entity.section.get` retrieves the list of sections from the application's data storage.

{% note info "" %}

The method works only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %}


## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Identifier of the application's data storage. Use the value specified when creating the storage.

You can obtain the identifier using the [entity.get](../entities/entity-get.md) method. ||
|| **SORT**
[`object`](../../data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n
}
```

where:
- `field_n` — sorting field
- `value_n` — sorting direction: `ASC` or `DESC`

Refer to the [Section Type](#section) for the list of available sorting fields.

By default, `{"ID":"ASC"}` is used.

Example: `{"NAME":"ASC","ID":"DESC"}` ||
|| **FILTER**
[`object`](../../data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n
}
```

where:
- `field_n` — filtering field
- `value_n` — filter value

Refer to the [Section Type](#section) for the list of available filtering fields.

You can add prefixes to the keys `field_n`:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `=` — equal (default)
- `!=` or `!` — not equal
- `><` — range
- `!><` — not in range
- `%` — LIKE
- `!%` — NOT LIKE
- `?` — check for `null`/`not null` ||
|| **start**
[`integer`](../../data-types.md) | Pagination parameter.

The page size is fixed at `50` records.

The formula to obtain the N-th page:
`start = (N - 1) * 50`

For more details, refer to the article [Features of List Methods](../../../settings/how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Example of retrieving sections from the storage, where:
- `ENTITY` — storage identifier `dish`
- `SORT` — sorting by name
- `FILTER` — only active sections

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ENTITY":"dish","SORT":{"NAME":"ASC"},"FILTER":{"ACTIVE":"Y"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/entity.section.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.section.get',
    		{
    			ENTITY: 'dish',
    			SORT: { NAME: 'ASC' },
    			FILTER: { ACTIVE: 'Y' },
    			start: 0,
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
                'entity.section.get',
                [
                    'ENTITY' => 'dish',
                    'SORT' => ['NAME' => 'ASC'],
                    'FILTER' => ['ACTIVE' => 'Y'],
                    'start' => 0,
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
        echo 'Error getting entity sections: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.section.get',
        {
            ENTITY: 'dish',
            SORT: { NAME: 'ASC' },
            FILTER: { ACTIVE: 'Y' },
            start: 0,
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
        'entity.section.get',
        [
            'ENTITY' => 'dish',
            'SORT' => ['NAME' => 'ASC'],
            'FILTER' => ['ACTIVE' => 'Y'],
            'start' => 0,
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
            "ID": "671",
            "CODE": null,
            "TIMESTAMP_X": "2026-03-23T17:15:51+02:00",
            "DATE_CREATE": "2026-03-23T17:15:51+02:00",
            "CREATED_BY": "577",
            "MODIFIED_BY": "577",
            "ACTIVE": "Y",
            "SORT": "500",
            "NAME": "Parent Section",
            "PICTURE": null,
            "DETAIL_PICTURE": null,
            "DESCRIPTION": null,
            "LEFT_MARGIN": "1",
            "RIGHT_MARGIN": "6",
            "DEPTH_LEVEL": "1",
            "ENTITY": "dish",
            "SECTION": null
        },
        {
            "ID": "669",
            "CODE": null,
            "TIMESTAMP_X": "2026-03-23T17:14:22+02:00",
            "DATE_CREATE": "2026-03-23T17:14:22+02:00",
            "CREATED_BY": "577",
            "MODIFIED_BY": "577",
            "ACTIVE": "Y",
            "SORT": "500",
            "NAME": "Test Section",
            "PICTURE": null,
            "DETAIL_PICTURE": null,
            "DESCRIPTION": null,
            "LEFT_MARGIN": "7",
            "RIGHT_MARGIN": "8",
            "DEPTH_LEVEL": "1",
            "ENTITY": "dish",
            "SECTION": null
        },
        {
            "ID": "673",
            "CODE": null,
            "TIMESTAMP_X": "2026-03-23T17:16:37+02:00",
            "DATE_CREATE": "2026-03-23T17:16:37+02:00",
            "CREATED_BY": "577",
            "MODIFIED_BY": "577",
            "ACTIVE": "Y",
            "SORT": "500",
            "NAME": "Test Section",
            "PICTURE": null,
            "DETAIL_PICTURE": null,
            "DESCRIPTION": null,
            "LEFT_MARGIN": "4",
            "RIGHT_MARGIN": "5",
            "DEPTH_LEVEL": "2",
            "ENTITY": "dish",
            "SECTION": "671"
        },
        {
            "ID": "675",
            "CODE": "test-section",
            "TIMESTAMP_X": "2026-03-23T17:42:32+02:00",
            "DATE_CREATE": "2026-03-23T17:42:32+02:00",
            "CREATED_BY": "577",
            "MODIFIED_BY": "577",
            "ACTIVE": "Y",
            "SORT": "500",
            "NAME": "Test Section",
            "PICTURE": null,
            "DETAIL_PICTURE": null,
            "DESCRIPTION": "Description of the test section",
            "LEFT_MARGIN": "2",
            "RIGHT_MARGIN": "3",
            "DEPTH_LEVEL": "2",
            "ENTITY": "dish",
            "SECTION": "671"
        }
    ],
    "total": 4,
    "time": {
        "start": 1774338416,
        "finish": 1774338416.415466,
        "duration": 0.4154660701751709,
        "processing": 0,
        "date_start": "2026-03-24T10:46:56+02:00",
        "date_finish": "2026-03-24T10:46:56+02:00",
        "operating_reset_at": 1774339016,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`section[]`](#section) | List of sections in the storage ||
|| **total**
[`integer`](../../data-types.md) | Total number of sections in the selection ||
|| **next**
[`integer`](../../data-types.md) | Offset for retrieving the next page (if available) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Section Type {#section}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the section ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the section ||
|| **TIMESTAMP_X**
[`datetime`](../../data-types.md) | Date and time of the last modification ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Date and time of creation ||
|| **CREATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who created the section ||
|| **MODIFIED_BY**
[`integer`](../../data-types.md) | Identifier of the user who modified the section ||
|| **ACTIVE**
[`string`](../../data-types.md) | Active flag (`Y` or `N`) ||
|| **SORT**
[`integer`](../../data-types.md) | Sorting index ||
|| **NAME**
[`string`](../../data-types.md) | Name of the section ||
|| **PICTURE**
[`string`](../../data-types.md) | URL of the section image or `null` ||
|| **DETAIL_PICTURE**
[`string`](../../data-types.md) | URL of the detailed section image or `null` ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Description of the section ||
|| **LEFT_MARGIN**
[`integer`](../../data-types.md) | Left boundary of the section in the tree ||
|| **RIGHT_MARGIN**
[`integer`](../../data-types.md) | Right boundary of the section in the tree ||
|| **DEPTH_LEVEL**
[`integer`](../../data-types.md) | Depth level of the section ||
|| **ENTITY**
[`string`](../../data-types.md) | Identifier of the storage ||
|| **SECTION**
[`integer`](../../data-types.md) | Identifier of the parent section or `null` ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "Argument 'ENTITY' is null or empty",
    "argument": "ENTITY"
}
```

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
|| `ERROR_ARGUMENT` | Argument 'ENTITY' is null or empty | The `ENTITY` parameter was not provided or is empty after cleaning ||
|| `ERROR_ARGUMENT` | Entity code is too long. Max length is N characters. | The `ENTITY` value is too long ||
|| `ERROR_ENTITY_NOT_FOUND` | Entity not found | The storage with the provided `ENTITY` was not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./entity-section-add.md)
- [{#T}](./entity-section-update.md)
- [{#T}](./entity-section-delete.md)
- [{#T}](./index.md)