# Get a List of Order Sources: sale.tradePlatform.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: user with access to the Online Store

The method `sale.tradePlatform.list` returns a list of order sources.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | The list of fields to return. If not passed or empty, the method returns all fields.

Possible values:
- `id` — order source ID
- `code` — order source code, see [Source Codes](#codes) for the values
- `active` — whether the source is active
- `name` — order source name
- `description` — order source description
- `settings` — order source settings
- `catalogSectionTabClassName` — tab class in the catalog section settings
- `class` — order source handler class
- `xmlId` — external ID

The fields are described in the [sale_order_trade_platform](../data-types.md#sale_order_trade_platform) object ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering in the format `{"field_1": "value_1", ... "field_N": "value_N"}`. The fields are the same as in `select`. Multiple conditions are combined with AND logic.

You can add a prefix to a field name:
- no prefix or `=` — equals
- `!=` — not equal
- `>`, `>=`, `<`, `<=` — greater than, greater than or equal to, less than, less than or equal to
- `@` — in the list, the value is passed as an array
- `!@` — not in the list, the value is passed as an array
- `%` — contains a substring, no `%` character is needed in the value
- `!%` — does not contain a substring
- `=%` or `%=` — LIKE by pattern, the `%` character is passed in the value: `"web%"` — starts with `web`
- `!=%` or `!%=` — NOT LIKE by pattern ||
|| **order**
[`object`](../../data-types.md) | An object for sorting in the format `{"field_1": "order_1", ... "field_N": "order_N"}`. The fields are the same as in `select`.

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order

By default, the list is sorted by `id` in ascending order ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for pagination control.

The page size of results is always static: 50 records.

To select the second page of results, pass the value `50`. To select the third page of results, pass `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number ||
|#

{% note warning "" %}

Pass field names in `select`, `filter`, and `order` in camelCase, as in the list above. The method silently skips fields with an unknown name or in uppercase: `select` returns all fields, and the filter condition and sorting are not applied.

{% endnote %}

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","code"],"filter":{"%code":"smart"},"order":{"code":"asc"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.tradePlatform.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","code"],"filter":{"%code":"smart"},"order":{"code":"asc"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.tradePlatform.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TradePlatformResult = {
      tradePlatforms: {
        id: number
        code: string
      }[]
    }

    try {
      // sale.tradePlatform.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<TradePlatformResult>({
        method: 'sale.tradePlatform.list',
        params: {
          select: ['id', 'code'],
          filter: { '%code': 'smart' },
          order: { code: 'asc' },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Trade platforms:', result.tradePlatforms, 'Count:', result.tradePlatforms.length)
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
      async function listTradePlatforms() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // sale.tradePlatform.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sale.tradePlatform.list',
            params: {
              select: ['id', 'code'],
              filter: { '%code': 'smart' },
              order: { code: 'asc' },
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
          console.info('Trade platforms:', result.tradePlatforms, 'Count:', result.tradePlatforms.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listTradePlatforms)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.sale.tradeplatform.list(
            select=[
                "id",
                "code",
            ],
            filter={
                "%code": "smart",
            },
            order={
                "code": "asc",
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
                'sale.tradePlatform.list',
                [
                    'select' => ['id', 'code'],
                    'filter' => ['%code' => 'smart'],
                    'order'  => ['code' => 'asc'],
                    'start'  => 0,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching trade platforms: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod( 
        "sale.tradePlatform.list", 
        {
            select: ['id', 'code'],
            filter: {'%code': 'smart'},
            order: {'code': 'asc'},
        }, 
        function(result) 
        { 
            if(result.error()) 
            {
                console.error(result);
            }
            else
            {
                console.dir(result);

                if (result.more())
                {
                    result.next();
                }
            } 
        } 
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.tradePlatform.list',
        [
            'select' => ['id', 'code'],
            'filter' => ['%code' => 'smart'],
            'order' => ['code' => 'asc'],
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
    res, err := client.Core().Call(ctx, "sale.tradePlatform.list", b24.Params{
    	"select": []string{"id", "code"},
    	"filter": b24.Params{
    		"%code": "smart",
    	},
    	"order": b24.Params{
    		"code": "asc",
    	},
    	"start": 0,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("sale.tradePlatform.list: %w", err)
    }

    // The method wraps the response in an object with the "tradePlatforms" key.
    raw, ok := b24.Unwrap(res.Result, "tradePlatforms")
    if !ok {
    	return fmt.Errorf("no tradePlatforms key in the response")
    }

    var items []struct {
    	Code string `json:"code"`
    	ID   b24.ID `json:"id"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.Code)
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "tradePlatforms": [
            {
                "code": "smart_invoice",
                "id": 31
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1712135957.057659,
        "finish": 1712135957.407821,
        "duration": 0.3501620292663574,
        "processing": 0.011919021606445312,
        "date_start": "2024-04-03T09:19:17+01:00",
        "date_finish": "2024-04-03T09:19:17+01:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **tradePlatforms**
[`sale_order_trade_platform[]`](../data-types.md#sale_order_trade_platform) | Order sources that match the filter. Each item contains the fields from `select` ||
|#

#### Source Codes {#codes}

The source type is determined by the `code` field.

#|
|| **Code** | **Source** ||
|| `deal` | Deal ||
|| `smart_invoice` | Invoice ||
|| `dynamic_{id}` | SPA with payments enabled, `{id}` — SPA type ID ||
|| `activity` | Activity ||
|| `realization_document` | Sales order ||
|| `terminal` | Terminal ||
|| `landing_{id}` | Landing page or online store on Sites, `{id}` — site ID ||
|| `webform_{id}` | CRM form, `{id}` — form ID ||
|#

A landing page or CRM form source is created separately for each site or form. To find all sources of one type, use the `%=` prefix in `filter`, for example `{"%=code": "landing_%"}`.

## Error Handling

HTTP Status: **400**

```json
{
    "error": "100",
    "error_description": "Invalid value {x} to match with parameter {filter}. Should be value of type array."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Invalid value {id} to match with parameter {select}. Should be value of type array. | A value that is not an array or object was passed in `select`, `filter`, or `order`. The error text contains the passed value and the parameter name ||
|| `400` | `200040300010` | Access Denied | The user has no access to the Online Store ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-trade-platform-get-fields.md)
