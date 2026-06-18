# Get a List of Storage Items: entity.item.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: a user authorized in the application

The `entity.item.get` method retrieves a list of items from the application's data storage.

{% note info "" %}

This method works only in the context of an [application](../../../settings/app-installation/index.md).

{% endnote %}

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

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

Refer to the [Item Type](#item) section for a list of available sorting fields.

By default, `{"ID":"ASC"}` is used. ||
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

Refer to the [Item Type](#item) section for a list of available filtering fields.

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

The formula for obtaining the N-th page:
`start = (N - 1) * 50`

For more details, refer to the article [Features of List Methods](../../../settings/how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

Example of retrieving storage items where:
- `ENTITY` — storage identifier `dish_v2`
- `SORT` — sorting by activity date and identifier
- `FILTER` — date range of activity

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ENTITY":"dish_v2","SORT":{"DATE_ACTIVE_FROM":"ASC","ID":"ASC"},"FILTER":{">=DATE_ACTIVE_FROM":"2026-03-01T00:00:00+01:00","<DATE_ACTIVE_FROM":"2026-04-01T00:00:00+01:00"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/entity.item.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each item returned in result[]
    type EntityItem = {
      ID: string
      TIMESTAMP_X: ISODate
      MODIFIED_BY: string
      DATE_CREATE: ISODate
      CREATED_BY: string
      ACTIVE: string
      DATE_ACTIVE_FROM: ISODate | string
      DATE_ACTIVE_TO: ISODate | string
      SORT: string
      NAME: string
      PREVIEW_PICTURE: string | null
      PREVIEW_TEXT: string | null
      DETAIL_PICTURE: string | null
      DETAIL_TEXT: string | null
      CODE: string | null
      ENTITY: string
      SECTION: string | null
      PROPERTY_VALUES?: Record<string, unknown>
    }

    try {
      // entity.item.get returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<EntityItem[]>({
        method: 'entity.item.get',
        params: {
          ENTITY: 'dish_v2',
          SORT: {
            DATE_ACTIVE_FROM: 'ASC',
            ID: 'ASC',
          },
          FILTER: {
            '>=DATE_ACTIVE_FROM': '2026-03-01T00:00:00+03:00',
            '<DATE_ACTIVE_FROM': '2026-04-01T00:00:00+03:00',
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Items count:', result.length, 'First item:', result[0])
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getEntityItems() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // entity.item.get returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'entity.item.get',
            params: {
              ENTITY: 'dish_v2',
              SORT: {
                DATE_ACTIVE_FROM: 'ASC',
                ID: 'ASC',
              },
              FILTER: {
                '>=DATE_ACTIVE_FROM': '2026-03-01T00:00:00+03:00',
                '<DATE_ACTIVE_FROM': '2026-04-01T00:00:00+03:00',
              },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Items count:', result.length, 'First item:', result[0])
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getEntityItems)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'entity.item.get',
                [
                    'ENTITY' => 'dish_v2',
                    'SORT' => [
                        'DATE_ACTIVE_FROM' => 'ASC',
                        'ID' => 'ASC',
                    ],
                    'FILTER' => [
                        '>=DATE_ACTIVE_FROM' => '2026-03-01T00:00:00+01:00',
                        '<DATE_ACTIVE_FROM' => '2026-04-01T00:00:00+01:00',
                    ],
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
        echo 'Error getting entity items: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.item.get',
        {
            ENTITY: 'dish_v2',
            SORT: {
                DATE_ACTIVE_FROM: 'ASC',
                ID: 'ASC',
            },
            FILTER: {
                '>=DATE_ACTIVE_FROM': '2026-03-01T00:00:00+01:00',
                '<DATE_ACTIVE_FROM': '2026-04-01T00:00:00+01:00',
            },
        },
        (result) => {
            if (result.error()) {
                console.error(result.error());
                return;
            }

            console.info(result.data());

            if (result.more()) {
                result.next();
            }
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'entity.item.get',
        [
            'ENTITY' => 'dish_v2',
            'SORT' => [
                'DATE_ACTIVE_FROM' => 'ASC',
                'ID' => 'ASC',
            ],
            'FILTER' => [
                '>=DATE_ACTIVE_FROM' => '2026-03-01T00:00:00+01:00',
                '<DATE_ACTIVE_FROM' => '2026-04-01T00:00:00+01:00',
            ],
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
            "ID": "2331",
            "TIMESTAMP_X": "2026-03-25T12:29:06+01:00",
            "MODIFIED_BY": "577",
            "DATE_CREATE": "2026-03-25T12:29:06+01:00",
            "CREATED_BY": "577",
            "ACTIVE": "Y",
            "DATE_ACTIVE_FROM": "",
            "DATE_ACTIVE_TO": "",
            "SORT": "500",
            "NAME": "Test Item",
            "PREVIEW_PICTURE": null,
            "PREVIEW_TEXT": null,
            "DETAIL_PICTURE": null,
            "DETAIL_TEXT": null,
            "CODE": null,
            "ENTITY": "dish",
            "SECTION": null
        }
    ],
    "total": 1,
    "time": {
        "start": 1774430946,
        "finish": 1774430946.627232,
        "duration": 0.6272320747375488,
        "processing": 0,
        "date_start": "2026-03-25T12:29:06+01:00",
        "date_finish": "2026-03-25T12:29:06+01:00",
        "operating_reset_at": 1774431546,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`item[]`](#item) | List of storage items ||
|| **total**
[`integer`](../../data-types.md) | Total number of items in the selection ||
|| **next**
[`integer`](../../data-types.md) | Offset for retrieving the next page (if available) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Item Type {#item}

#| 
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the item ||
|| **TIMESTAMP_X**
[`datetime`](../../data-types.md) | Date and time of the last modification ||
|| **MODIFIED_BY**
[`integer`](../../data-types.md) | Identifier of the user who modified the item ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Date and time of creation ||
|| **CREATED_BY**
[`integer`](../../data-types.md) | Identifier of the user who created the item ||
|| **ACTIVE**
[`string`](../../data-types.md) | Active flag (`Y` or `N`) ||
|| **DATE_ACTIVE_FROM**
[`datetime`](../../data-types.md) \| [`string`](../../data-types.md) | Start date of activity or empty string ||
|| **DATE_ACTIVE_TO**
[`datetime`](../../data-types.md) \| [`string`](../../data-types.md) | End date of activity or empty string ||
|| **SORT**
[`integer`](../../data-types.md) | Sorting index ||
|| **NAME**
[`string`](../../data-types.md) | Name of the item ||
|| **PREVIEW_PICTURE**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | URL of the preview image ||
|| **PREVIEW_TEXT**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Preview text ||
|| **DETAIL_PICTURE**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | URL of the detailed image ||
|| **DETAIL_TEXT**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Detailed text ||
|| **CODE**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Symbolic code of the item ||
|| **ENTITY**
[`string`](../../data-types.md) | Identifier of the storage ||
|| **SECTION**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | Identifier of the section ||
|| **PROPERTY_VALUES**
[`object`](../../data-types.md) | Object of property values in the format `{"PROPERTY_CODE": value}`. The field is present if the storage has properties.

To filter by a property value, use the key `PROPERTY_<PROPERTY_CODE>`, for example `{"PROPERTY_PRICE": 100}`.

You can obtain a list of available property codes using the [entity.item.property.get](./properties/entity-item-property-get.md) method. ||
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
|| `ERROR_ARGUMENT` | Argument 'ENTITY' is null or empty | The `ENTITY` parameter is not provided or is empty after cleanup. ||
|| `ERROR_ARGUMENT` | Entity code is too long. Max length is N characters. | The `ENTITY` value is too long. ||
|| `ERROR_ARGUMENT` | Filter validator errors | Invalid values were provided for the `FILTER` parameter. ||
|| `ERROR_ENTITY_NOT_FOUND` | Entity not found | The storage with the provided `ENTITY` was not found. ||
|| `ACCESS_DENIED` | Access denied! Application context required | No application context (`clientId`). ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./entity-item-add.md)
- [{#T}](./entity-item-update.md)
- [{#T}](./entity-item-delete.md)
- [{#T}](./properties/index.md)
