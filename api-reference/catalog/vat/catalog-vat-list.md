# Get a List of VAT Rates by Filter catalog.vat.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "Manage tax rates" access permission

The method `catalog.vat.list` returns a list of VAT rates with filtering, sorting, and pagination.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select** 
[`array`](../../data-types.md)| An array of VAT rate fields to return. Available fields: `active`, `id`, `name`, `rate`, `sort`, `timestampX`. The fields are described in the [catalog_vat](../data-types.md#catalog_vat) object.

If the array is not provided or is empty, the method returns all fields. The method skips unknown fields without an error
||
|| **filter** 
[`object`](../../data-types.md)| An object for filtering the selected VAT rates in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Possible values for `field`: `active`, `id`, `name`, `rate`, `sort`, `timestampX`. For example, `{"active": "N"}` returns only inactive rates.

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as the value)
- `!@` — NOT IN (an array is passed as the value)
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
  - `"mol%"` — searching for values starting with "mol"
  - `"%mol"` — searching for values ending with "mol"
  - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search is performed on both sides
- `!=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
  - `"mol%"` — searching for values not starting with "mol"
  - `"%mol"` — searching for values not ending with "mol"
  - `"%mol%"` — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

The method skips unknown fields in the filter without an error and returns rates without applying such a condition. Check the field names against the list above
||
|| **order**
[`object`](../../data-types.md)| An object for sorting the selected VAT rates in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field`: `active`, `id`, `name`, `rate`, `sort`, `timestampX`.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order

If `order` is not provided, the method does not set an explicit sort order. For a stable order when paging through results, pass a sort order, for example `{"id": "asc"}`. The method skips unknown fields in `order` without an error
||
|| **start** 
[`integer`](../../data-types.md)| This parameter is used for managing pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the value of the `start` parameter:

`start = (N-1) * 50`, where `N` — the desired page number
||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","rate"],"filter":{"active":"Y"},"order":{"id":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.vat.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","rate"],"filter":{"active":"Y"},"order":{"id":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.vat.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CatalogVatListResult = {
      vats: {
        id: number
        name: string
        rate: number | null
      }[]
    }

    // catalog.vat.list returns a single page (max 50 records). For the whole result set
    // use a list helper: $b24.actions.v2.callList.make() returns every record as one
    // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
    // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
    // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
    try {
      const response = await $b24.actions.v2.call.make<CatalogVatListResult>({
        method: 'catalog.vat.list',
        params: {
          select: [
            'id',
            'name',
            'rate',
          ],
          filter: {
            active: 'Y',
          },
          order: {
            id: 'ASC',
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
        console.info('VAT rates:', result.vats, 'count:', result.vats.length)
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
      async function fetchVatList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // catalog.vat.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'catalog.vat.list',
            params: {
              select: [
                'id',
                'name',
                'rate',
              ],
              filter: {
                active: 'Y',
              },
              order: {
                id: 'ASC',
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
          console.info('VAT rates:', result.vats, 'count:', result.vats.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchVatList)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.vat.list(
            select=[
                "id",
                "name",
                "rate",
            ],
            filter={
                "active": "Y",
            },
            order={
                "id": "ASC",
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
                'catalog.vat.list',
                [
                    'select' => [
                        'id',
                        'name',
                        'rate'
                    ],
                    'filter' => [
                        'active' => 'Y'
                    ],
                    'order' => [
                        'id' => "ASC"
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result['vats'], true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing VAT: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.vat.list',
        {
            select: [
                'id',
                'name',
                'rate'
            ],
            filter: {
                active: 'Y'
            },
            order: {
                'id': "ASC"
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.log(result.data());
                if(result.more())
                    result.next();
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.vat.list',
        [
            'select' => [
                'id',
                'name',
                'rate'
            ],
            'filter' => [
                'active' => 'Y'
            ],
            'order' => [
                'id' => 'ASC'
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
    res, err := client.Core().Call(ctx, "catalog.vat.list", b24.Params{
    	"select": []string{"id", "name", "rate"},
    	"filter": b24.Params{
    		"active": "Y",
    	},
    	"order": b24.Params{
    		"id": "ASC",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.vat.list: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "vats": [
            {
                "id": 1,
                "name": "No VAT",
                "rate": null
            },
            {
                "id": 3,
                "name": "VAT 19%",
                "rate": 19
            },
            {
                "id": 7,
                "name": "VAT 7%",
                "rate": 7
            }
        ]
    },
    "total": 3,
    "time": {
        "start": 1793118107,
        "finish": 1793118107.896136,
        "duration": 0.8961360454559326,
        "processing": 0,
        "date_start": "2026-10-27T17:21:47+01:00",
        "date_finish": "2026-10-27T17:21:47+01:00",
        "operating_reset_at": 1793118660,
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
|| **vats**
[`catalog_vat[]`](../data-types.md#catalog_vat) | Array of VAT rates on the current page, up to 50 items. Each item contains the fields from `select`. For a rate without VAT, the `rate` field is `null` ||
|| **total**
[`integer`](../../data-types.md) | Total number of rates matching the filter. If `total` is greater than 50, retrieve the next pages using the `start` parameter ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "200040300010",
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user has neither the "View product catalog" nor the "Manage tax rates" access permission ||
|| — | `0` | — | Other errors, for example, fatal errors ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./catalog-vat-add.md)
- [{#T}](./catalog-vat-update.md)
- [{#T}](./catalog-vat-get.md)
- [{#T}](./catalog-vat-delete.md)
- [{#T}](./catalog-vat-get-fields.md)

