# Add VAT Rate catalog.vat.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Manage tax rates" access permission

The method `catalog.vat.add` creates a VAT rate.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating a new VAT rate ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Name of the VAT rate, for example `VAT 7%` ||
|| **active**
[`string`](../../data-types.md) | Indicator of the VAT rate's activity. Possible values:
- `Y` — active
- `N` — inactive

Default is `Y`
||
|| **rate***
[`double`](../../data-types.md) | Rate value in percent, for example `7` or `19`. The value `null` is not accepted: a rate without VAT cannot be created using this method ||
|| **sort**
[`integer`](../../data-types.md) | Sort order of the rate in lists. The lower the value, the higher the rate appears in the list.

Default is `100`
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"VAT 7%","rate":7,"sort":500,"active":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.vat.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"VAT 7%","rate":7,"sort":500,"active":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.vat.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CatalogVatAddResult = {
      vat: {
        active: string
        id: number
        name: string
        rate: number
        sort: number
        timestampX: ISODate
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<CatalogVatAddResult>({
        method: 'catalog.vat.add',
        params: {
          fields: {
            name: 'VAT 7%',
            rate: 7,
            sort: 500,
            active: 'Y',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Added VAT rate:', result.vat.id, result.vat.name, result.vat.rate)
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
      async function addVatRate() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.vat.add',
            params: {
              fields: {
                name: 'VAT 7%',
                rate: 7,
                sort: 500,
                active: 'Y',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Added VAT rate:', result.vat.id, result.vat.name, result.vat.rate)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addVatRate)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.vat.add(
            fields={
                "name": "VAT 7%",
                "rate": 7,
                "sort": 500,
                "active": "Y",
            },
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
                'catalog.vat.add',
                [
                    'fields' => [
                        'name'   => 'VAT 7%',
                        'rate'   => 7,
                        'sort'   => 500,
                        'active' => "Y",
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result['vat']);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
    'catalog.vat.add',
            {
                fields: {
                        name: 'VAT 7%',
                        rate: 7,
                        sort: 500,
                        active: "Y",
                }
            },
    function(result) {
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
        'catalog.vat.add',
        [
            'fields' => [
                'name' => 'VAT 7%',
                'rate' => 7,
                'sort' => 500,
                'active' => "Y"
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
    res, err := client.Core().Call(ctx, "catalog.vat.add", b24.Params{
    	"fields": b24.Params{
    		"name":   "VAT 7%",
    		"rate":   7,
    		"sort":   500,
    		"active": "Y",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.vat.add: %w", err)
    }

    // The method wraps the response in an object with the "vat" key.
    raw, ok := b24.Unwrap(res.Result, "vat")
    if !ok {
    	return fmt.Errorf("no vat key in the response")
    }

    var item struct {
    	Active     string  `json:"active"`
    	ID         b24.ID  `json:"id"`
    	Name       string  `json:"name"`
    	Rate       float64 `json:"rate"`
    	Sort       int     `json:"sort"`
    	TimestampX string  `json:"timestampX"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Active, item.ID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "vat": {
            "active": "Y",
            "id": 37,
            "name": "VAT 7%",
            "rate": 7,
            "sort": 500,
            "timestampX": "2026-10-27T17:17:29+01:00"
        }
    },
    "time": {
        "start": 1793117849,
        "finish": 1793117849.807201,
        "duration": 0.8072009086608887,
        "processing": 0,
        "date_start": "2026-10-27T17:17:29+01:00",
        "date_finish": "2026-10-27T17:17:29+01:00",
        "operating_reset_at": 1793118420,
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
|| **vat**
[`catalog_vat`](../data-types.md#catalog_vat) | The created VAT rate with all fields: `active`, `id`, `name`, `rate`, `sort`, `timestampX`. Use the `id` value in the `vatId` field of a trade catalog or product ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Required fields: rate"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300020` | Access Denied | The user does not have the "Manage tax rates" access permission ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter is not provided ||
|| `400` | `0` | Required fields: name | The `name` field is missing in `fields` ||
|| `400` | `0` | Required fields: rate | The `rate` field is missing in `fields` or `null` is passed ||
|| `400` | `0` | Error adding VAT | Failed to save the VAT rate ||
|| — | `0` | — | Other errors, for example, fatal errors ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./catalog-vat-update.md)
- [{#T}](./catalog-vat-get.md)
- [{#T}](./catalog-vat-list.md)
- [{#T}](./catalog-vat-delete.md)
- [{#T}](./catalog-vat-get-fields.md)
- [{#T}](../catalog/catalog-catalog-update.md)
- [{#T}](../product/catalog-product-update.md)