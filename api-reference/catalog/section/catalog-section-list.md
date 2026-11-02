# Get a List of Sections in the Trade Catalog catalog.section.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "View catalog products" permission and permission to read the information block sections

The method `catalog.section.list` returns a list of sections in the trade catalog.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to select (see the fields of the [catalog_section](../data-types.md#catalog_section) object).

If not provided or an empty array is passed, all available fields of the catalog section will be selected. ||
|| **filter***
[`object`](../../data-types.md) | Object for filtering catalog sections in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

The required key is `iblockId`, the identifier of the trade catalog information block. You can get it using the [catalog.catalog.list](../catalog/catalog-catalog-list.md) method.

You can filter by the `id`, `iblockId`, `iblockSectionId`, `name`, `code`, `xmlId`, and `active` fields. The method skips conditions on other fields, for example `sort` or `description`, without an error.

You can add a prefix to the key that sets the condition:
- `=` — equals. Used by default. If you pass an array of values, the method returns sections whose value is in the array
- `!` or `!=` — not equal. With an array of values — not in the array
- `>` — greater than
- `>=` — greater than or equal to
- `<` — less than
- `<=` — less than or equal to
- `%` — contains a substring. Do not pass the `%` character in the value

The method does not support the `@`, `!@`, `=%`, `%=`, `!%`, `!=%`, and `!%=` prefixes: conditions with them are not applied ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected catalog sections in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_section](../data-types.md#catalog_section).

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to control pagination.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "select": ["id", "name", "sort"],
        "filter": {
            "iblockId": 25,
            "iblockSectionId": 31
        },
        "order": {
            "sort": "DESC"
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.section.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "select": ["id", "name", "sort"],
        "filter": {
            "iblockId": 25,
            "iblockSectionId": 31
        },
        "order": {
            "sort": "DESC"
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/catalog.section.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CatalogSectionListResult = {
      sections: {
        id: number
        name: string
        sort: number
      }[]
    }

    try {
      // catalog.section.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<CatalogSectionListResult>({
        method: 'catalog.section.list',
        params: {
          select: ['id', 'name', 'sort'],
          filter: {
            iblockId: 25,
            'iblockSectionId': 31,
          },
          order: {
            sort: 'DESC',
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
        console.info('Sections on this page:', result.sections.length, result.sections)
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
      async function fetchCatalogSections() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // catalog.section.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'catalog.section.list',
            params: {
              select: ['id', 'name', 'sort'],
              filter: {
                iblockId: 25,
                'iblockSectionId': 31,
              },
              order: {
                sort: 'DESC',
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
          console.info('Sections on this page:', result.sections.length, result.sections)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchCatalogSections)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.section.list(
            select=[
                "id",
                "name",
                "sort",
            ],
            filter={
                "iblockId": 25,
            },
            order={
                "sort": "DESC",
            },
            start=0,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP
    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.section.list',
                [
                    'select' => ["id", "name", "sort"],
                    'filter' => [
                        'iblockId' => 25,
                        'iblockSectionId'  => 31
                    ],
                    'order'  => ['sort' => 'DESC']
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.section.list',
        {
            select: ["id", "name", "sort"],
            filter: {
                'iblockId': 25,
                'iblockSectionId': 31
            },
            order: {'sort': 'DESC'}
        },
        function(result)
        {
        if(result.error())
            console.error(result.error());
        else
            console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.section.list',
        [
            'select' => ["id", "name", "sort"],
            'filter' => [
                'iblockId' => 25,
                'iblockSectionId' => 31
            ],
            'order' => [
                'sort' => 'DESC'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.section.list", b24.Params{
    	"select": []string{"id", "name", "sort"},
    	"filter": b24.Params{
    		"iblockId": 25,
    		"iblockSectionId":   31,
    	},
    	"order": b24.Params{
    		"sort": "DESC",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.section.list: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "sections": [
            {
                "id": 19,
                "name": "Excursions",
                "sort": 500
            },
            {
                "id": 21,
                "name": "Collectible spoons",
                "sort": 500
            },
            {
                "id": 43,
                "name": "Sportswear",
                "sort": 500
            },
            {
                "id": 41,
                "name": "T-shirts",
                "sort": 400
            },
            {
                "id": 39,
                "name": "Dresses",
                "sort": 100
            },
            {
                "id": 33,
                "name": "Shoes",
                "sort": 50
            }
        ]
    },
    "total": 6,
    "time": {
        "start": 1793615638,
        "finish": 1793615638.10939,
        "duration": 0.1093900203704834,
        "processing": 0,
        "date_start": "2026-11-02T13:33:58+03:00",
        "date_finish": "2026-11-02T13:33:58+03:00",
        "operating_reset_at": 1793616180,
        "operating": 0.6537201404571533
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **result.sections**
[`catalog_section[]`](../data-types.md#catalog_section) | An array of objects containing information about the selected catalog sections ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Required filter fields: iblockId"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | No "View product catalog" or "View catalog products" permission ||
|| `400` | `200040300030` | Access Denied | No permission to read the information block from `filter.iblockId` ||
|| `400` | `200700300050` | Iblock is not exists | The information block from `filter.iblockId` was not found ||
|| `400` | `0` | Required filter fields: iblockId | The required key `iblockId` is not passed in `filter` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-section-add.md)
- [{#T}](./catalog-section-update.md)
- [{#T}](./catalog-section-get.md)
- [{#T}](./catalog-section-delete.md)
- [{#T}](./catalog-section-get-fields.md)
