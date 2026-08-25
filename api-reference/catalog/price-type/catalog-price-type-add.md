# Create Price Type catalog.priceType.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a new price type.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md)| Field values for creating a new price type ([detailed description](#fields)) ||
|#

### Field Parameter {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Code of the price type.

To ensure the stable operation of internal services, the price type code must be specified using only English characters.
||
|| **base**
[`string`](../../data-types.md) | Indicates whether the price type is base. Possible values:
- `Y` — yes
- `N` — no

Default is `N`.

Only one base price type can exist at a time. When a new base type is added, the previous one will lose this property and cease to be base.
||
|| **sort**
[`integer`](../../data-types.md) | Sorting.

Default is `100`.
||
|| **xmlId**
[`string`](../../data-types.md) | External code.

Can be used to synchronize the current price type with a similar position in an external system.
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
    -d '{"fields":{"name":"Wholesale price","base":"N","sort":10,"xmlId":"wholesale"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.priceType.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Wholesale price","base":"N","sort":10,"xmlId":"wholesale"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.priceType.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type AddPriceTypeResult = {
      priceType: {
        base: string
        createdBy: number
        dateCreate: ISODate | null
        id: number
        modifiedBy: number
        name: string
        sort: number
        timestampX: ISODate | null
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<AddPriceTypeResult>({
        method: 'catalog.priceType.add',
        params: {
          fields: {
            name: 'Wholesale price',
            base: 'N',
            sort: 10,
            xmlId: 'wholesale',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created price type id:', result.priceType.id, 'name:', result.priceType.name)
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
      async function addPriceType() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.priceType.add',
            params: {
              fields: {
                name: 'Wholesale price',
                base: 'N',
                sort: 10,
                xmlId: 'wholesale',
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
          console.info('Created price type id:', result.priceType.id, 'name:', result.priceType.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addPriceType)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.price_type.add(
            fields={
                "name": "Wholesale price",
                "base": "N",
                "sort": 10,
                "xmlId": "wholesale",
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
                'catalog.priceType.add',
                [
                    'fields' => [
                        'name'  => "Wholesale price",
                        'base'  => "N",
                        'sort'  => 10,
                        'xmlId' => "wholesale",
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error()->ex);
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding price type: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.priceType.add', 
        {
            fields: {
                name: "Wholesale price",
                base: "N",
                sort: 10,
                xmlId: "wholesale"
            }
        },
        function(result) {
            if (result.error())
                console.error(result.error().ex);
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.priceType.add',
        [
            'fields' => [
                'name' => 'Wholesale price',
                'base' => 'N',
                'sort' => 10,
                'xmlId' => 'wholesale'
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
    res, err := client.Core().Call(ctx, "catalog.priceType.add", b24.Params{
    	"fields": b24.Params{
    		"name":  "Wholesale price",
    		"base":  "N",
    		"sort":  10,
    		"xmlId": "wholesale",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.priceType.add: %w", err)
    }

    // The method wraps the response in an object with the "priceType" key.
    raw, ok := b24.Unwrap(res.Result, "priceType")
    if !ok {
    	return fmt.Errorf("no priceType key in the response")
    }

    var item struct {
    	Base       string `json:"base"`
    	CreatedBy  int    `json:"createdBy"`
    	DateCreate string `json:"dateCreate"`
    	ID         b24.ID `json:"id"`
    	ModifiedBy int    `json:"modifiedBy"`
    	Name       string `json:"name"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Base, item.CreatedBy)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "priceType": {
            "base": "N",
            "createdBy": 1,
            "dateCreate": "2024-10-02T17:49:44+02:00",
            "id": 2,
            "modifiedBy": 1,
            "name": "Wholesale price",
            "sort": 10,
            "timestampX": "2024-10-02T17:49:44+02:00",
            "xmlId": "wholesale"
        }
    },
    "time": {
        "start": 1716552521.40908,
        "finish": 1716552521.69852,
        "duration": 0.289434909820557,
        "processing": 0.011207103729248,
        "date_start": "2024-10-02T17:49:44+02:00",
        "date_finish": "2024-10-02T17:49:44+02:00",
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
|| **priceType**
[`catalog_price_type`](../data-types.md#catalog_price_type) | Object with information about the created price type ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 200040300020,
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300020` | Insufficient permissions to edit
||
|| `100` | Required parameter `fields` not provided
||
|| `0` | Required fields not set
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-price-type-update.md)
- [{#T}](./catalog-price-type-get.md)
- [{#T}](./catalog-price-type-list.md)
- [{#T}](./catalog-price-type-delete.md)
- [{#T}](./catalog-price-type-get-fields.md)