# Get a List of Warehouses catalog.store.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "View and select warehouses" access permission

The method `catalog.store.list` returns a list of warehouses with filtering, sorting, and pagination.

The response includes only the warehouses available to the user through the "View and select warehouses" access permission, and the warehouses the user created if they have the "Edit warehouses" access permission.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of warehouse fields to return. Available fields: `active`, `address`, `code`, `dateCreate`, `dateModify`, `description`, `email`, `gpsN`, `gpsS`, `id`, `imageId`, `issuingCenter`, `modifiedBy`, `phone`, `schedule`, `sort`, `title`, `userId`, `xmlId`. The fields are described in the [catalog_store](../data-types.md#catalog_store) object.

If the array is not provided or is empty, the method returns all fields. The method ignores unknown fields without an error. Write field names in camelCase: if `select` contains no known field, for example, `["ID", "TITLE"]`, the method returns all fields
||
|| **filter**
[`object`](../../data-types.md) | An object for filtering warehouses in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Possible values for `field` are the fields listed in `select`. For example, `{"active": "Y"}` returns only active warehouses.

An additional prefix can be specified for the key to clarify the filter's behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search goes from both sides
- `!=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

An array without a prefix works as IN: `{"id": [33, 35]}` returns both warehouses.

The method ignores unknown fields and uppercase names, for example, `ID`, without an error and returns warehouses without applying such a condition. Check field names against the list above
||
|| **order**
[`object`](../../data-types.md) | An object for sorting warehouses in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` are the fields listed in `select`.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order

The direction is case-insensitive: `DESC` and `desc` work the same way. The method ignores unknown fields in `order` without an error
||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for managing pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page.

If `start` is greater than the number of warehouses found, the method returns an empty `stores` array
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

The example retrieves active warehouses whose symbolic code contains `kgd` and whose sort order is at least `200`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","title","address","active","issuingCenter","sort","code","xmlId"],"filter":{"active":"Y",">=sort":200,"%code":"kgd"},"order":{"sort":"asc","id":"desc"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.store.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","title","address","active","issuingCenter","sort","code","xmlId"],"filter":{"active":"Y",">=sort":200,"%code":"kgd"},"order":{"sort":"asc","id":"desc"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.store.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of one store returned in result.stores (only the selected fields)
    type StoreListItem = {
      id: number
      title: string | null
      address: string
      active: string
      issuingCenter: string
      sort: number | null
      code: string | null
      xmlId: string | null
    }

    try {
      // catalog.store.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<{ stores: StoreListItem[] }>({
        method: 'catalog.store.list',
        params: {
          select: ['id', 'title', 'address', 'active', 'issuingCenter', 'sort', 'code', 'xmlId'],
          filter: {
            active: 'Y',
            '>=sort': 200,
            '%code': 'kgd',
          },
          order: {
            sort: 'asc',
            id: 'desc',
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
        console.info('Stores on page:', result.stores.length)
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
      async function listStores() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.store.list',
            params: {
              select: ['id', 'title', 'address', 'active', 'issuingCenter', 'sort', 'code', 'xmlId'],
              filter: {
                active: 'Y',
                '>=sort': 200,
                '%code': 'kgd',
              },
              order: {
                sort: 'asc',
                id: 'desc',
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
          console.info('Stores on page:', result.stores.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listStores)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.store.list(
            select=["id", "title", "address", "active", "issuingCenter", "sort", "code", "xmlId"],
            filter={
                "active": "Y",
                ">=sort": 200,
                "%code": "kgd",
            },
            order={
                "sort": "asc",
                "id": "desc",
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
                'catalog.store.list',
                [
                    'select' => ['id', 'title', 'address', 'active', 'issuingCenter', 'sort', 'code', 'xmlId'],
                    'filter' => [
                        'active' => 'Y',
                        '>=sort' => 200,
                        '%code'  => 'kgd',
                    ],
                    'order'  => [
                        'sort' => 'asc',
                        'id'   => 'desc',
                    ],
                    'start'  => 0,
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
        'catalog.store.list',
        {
            select: ['id', 'title', 'address', 'active', 'issuingCenter', 'sort', 'code', 'xmlId'],
            filter: {
                active: 'Y',
                '>=sort': 200,
                '%code': 'kgd',
            },
            order: {
                sort: 'asc',
                id: 'desc',
            },
            start: 0,
        },
        function(result)
        {
            if (result.error())
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
        'catalog.store.list',
        [
            'select' => ['id', 'title', 'address', 'active', 'issuingCenter', 'sort', 'code', 'xmlId'],
            'filter' => [
                'active' => 'Y',
                '>=sort' => 200,
                '%code' => 'kgd',
            ],
            'order' => [
                'sort' => 'asc',
                'id' => 'desc',
            ],
            'start' => 0,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.store.list", b24.Params{
    	"select": []string{"id", "title", "address", "active", "issuingCenter", "sort", "code", "xmlId"},
    	"filter": b24.Params{
    		"active": "Y",
    		">=sort": 200,
    		"%code":  "kgd",
    	},
    	"order": b24.Params{
    		"sort": "asc",
    		"id":   "desc",
    	},
    	"start": 0,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.store.list: %w", err)
    }

    // The method wraps the response in an object with the "stores" key.
    raw, ok := b24.Unwrap(res.Result, "stores")
    if !ok {
    	return fmt.Errorf("the response has no stores key")
    }

    // Optional fields can be null, so they use pointers.
    var items []struct {
    	ID            b24.ID  `json:"id"`
    	Title         *string `json:"title"`
    	Address       string  `json:"address"`
    	Active        string  `json:"active"`
    	IssuingCenter string  `json:"issuingCenter"`
    	Sort          *int    `json:"sort"`
    	Code          *string `json:"code"`
    	XMLID         *string `json:"xmlId"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.Address)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "stores": [
            {
                "active": "Y",
                "address": "Tiergartenstraße 17, 10785 Berlin, Germany",
                "code": "store_kgd",
                "id": 37,
                "issuingCenter": "N",
                "sort": 200,
                "title": "Tiergarten Warehouse",
                "xmlId": "store-kgd-01"
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1793874021,
        "finish": 1793874021.852705,
        "duration": 0.8527050018310547,
        "processing": 0,
        "date_start": "2026-11-05T13:20:21+03:00",
        "date_finish": "2026-11-05T13:20:21+03:00",
        "operating_reset_at": 1793874600,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **result.stores**
[`catalog_store[]`](../data-types.md#catalog_store) | An array of warehouses. Each warehouse contains only the fields from `select`; empty fields come with the `null` value ||
|| **total**
[`integer`](../../data-types.md) | The total number of warehouses found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "100",
    "error_description": "Invalid order \"BAD\""
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user has neither the "View product catalog" nor the "View and select warehouses" access permission ||
|| `400` | `100` | Invalid order "BAD" | A direction other than `asc` and `desc` is passed in `order`. The text shows the direction in uppercase ||
|| `400` | `100` | Invalid value {abc} to match with parameter {filter}. Should be value of type array. | A value other than an object is passed in `filter`. The same error with `{select}` means that a value other than an array is passed in `select` ||
|| `400` | `0` | Wrong format value of filter field `id`. | A nested object is passed in the filter value. The text contains the filter key ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-store-add.md)
- [{#T}](./catalog-store-update.md)
- [{#T}](./catalog-store-get.md)
- [{#T}](./catalog-store-delete.md)
- [{#T}](./catalog-store-get-fields.md)
