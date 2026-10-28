# Update VAT Rate catalog.vat.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Manage tax rates" access permission

The method `catalog.vat.update` updates a VAT rate.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_vat.id`](../data-types.md#catalog_vat) | Identifier of the VAT rate.

To retrieve the rate identifiers, use the [catalog.vat.list](./catalog-vat-list.md) method
||
|| **fields***
[`object`](../../data-types.md) | Field values for updating the VAT rate ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Name of the VAT rate. Pass it in every call: without `name`, the method returns the `Required fields: name` error. To keep the name unchanged, pass the current value from [catalog.vat.get](./catalog-vat-get.md) ||
|| **active**
[`string`](../../data-types.md) | Indicator of the VAT rate's activity. Possible values:
- `Y` — active
- `N` — inactive

If not passed, the value does not change
||
|| **rate***
[`double`](../../data-types.md) | Rate value in percent, for example `7` or `19`. Pass it in every call: without `rate`, the method returns the `Required fields: rate` error ||
|| **sort**
[`integer`](../../data-types.md) | Sort order of the rate in lists. If not passed, the value does not change
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
    -d '{"id":37,"fields":{"name":"VAT 19%","rate":19,"sort":510,"active":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.vat.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":37,"fields":{"name":"VAT 19%","rate":19,"sort":510,"active":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.vat.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CatalogVatUpdateResult = {
      vat: {
        active: string
        id: number
        name: string
        rate: number
        sort: number
        timestampX: ISODate | null
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<CatalogVatUpdateResult>({
        method: 'catalog.vat.update',
        params: {
          id: 37,
          fields: {
            name: 'VAT 19%',
            rate: 19,
            sort: 510,
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
        console.info(result.vat)
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
      async function updateVat() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.vat.update',
            params: {
              id: 37,
              fields: {
                name: 'VAT 19%',
                rate: 19,
                sort: 510,
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
          console.info(result.vat)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateVat)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.vat.update(
            bitrix_id=37,
            fields={
                "name": "VAT 19%",
                "rate": 19,
                "sort": 510,
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
                'catalog.vat.update',
                [
                    'id' => 37,
                    'fields' => [
                        'name'   => "VAT 19%",
                        'rate'   => 19,
                        'sort'   => 510,
                        'active' => "Y"
                    ]
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
        'catalog.vat.update', 
        {
            id: 37,
            fields: {
                name: "VAT 19%",
                rate: 19,
                sort: 510,
                active: "Y"
            }
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
        'catalog.vat.update',
        [
            'id' => 37,
            'fields' => [
                'name' => 'VAT 19%',
                'rate' => 19,
                'sort' => 510,
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
    res, err := client.Core().Call(ctx, "catalog.vat.update", b24.Params{
    	"id": 37,
    	"fields": b24.Params{
    		"name":   "VAT 19%",
    		"rate":   19,
    		"sort":   510,
    		"active": "Y",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.vat.update: %w", err)
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
            "name": "VAT 19%",
            "rate": 19,
            "sort": 510,
            "timestampX": "2026-10-27T17:23:50+01:00"
        }
    },
    "time": {
        "start": 1793118230,
        "finish": 1793118230.267268,
        "duration": 0.26726794242858887,
        "processing": 0,
        "date_start": "2026-10-27T17:23:50+01:00",
        "date_finish": "2026-10-27T17:23:50+01:00",
        "operating_reset_at": 1793118780,
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
[`catalog_vat`](../data-types.md#catalog_vat) | The VAT rate after the update with all fields: `active`, `id`, `name`, `rate`, `sort`, `timestampX` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Required fields: name"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300020` | Access Denied | The user does not have the "Manage tax rates" access permission ||
|| `400` | `200800000000` | vat does not exist. | No VAT rate with this `id` exists ||
|| `400` | `100` | Could not find value for parameter {id} | The `id` parameter is not provided ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter is not provided ||
|| `400` | `0` | Required fields: name | The `name` field is missing in `fields` ||
|| `400` | `0` | Required fields: rate | The `rate` field is missing in `fields` ||
|| `400` | `0` | Error updating VAT | Failed to save the VAT rate changes ||
|| — | `0` | — | Other errors, for example, fatal errors ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./catalog-vat-add.md)
- [{#T}](./catalog-vat-get.md)
- [{#T}](./catalog-vat-list.md)
- [{#T}](./catalog-vat-delete.md)
- [{#T}](./catalog-vat-get-fields.md)