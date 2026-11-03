# Create Price Type catalog.priceType.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Manage Price Types" access permission

The method `catalog.priceType.add` adds a new price type.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Object with the fields of the new price type [(detailed description)](#fields) ||
|#

### Field Parameter {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Price type code. The value must not be empty
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

After creating a price type, the method automatically adds view (`access: "N"`) and purchase (`access: "Y"`) permissions for customer groups with the identifiers `1` and `2`. You can change the set of bindings using the [catalog.priceTypeGroup.*](./price-type-group/index.md) methods.

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

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
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
            "createdBy": 1269,
            "dateCreate": "2026-11-03T12:44:53+03:00",
            "id": 35,
            "modifiedBy": 1269,
            "name": "Wholesale price",
            "sort": 10,
            "timestampX": "2026-11-03T12:44:53+03:00",
            "xmlId": "wholesale"
        }
    },
    "time": {
        "start": 1793699093,
        "finish": 1793699093.715093,
        "duration": 0.7150928974151611,
        "processing": 0,
        "date_start": "2026-11-03T12:44:53+03:00",
        "date_finish": "2026-11-03T12:44:53+03:00",
        "operating_reset_at": 1793699640,
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
|| **result.priceType**
[`catalog_price_type`](../data-types.md#catalog_price_type) | Object with information about the created price type ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
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
|| `400` | `200040300020` | Access Denied | The user does not have the "Manage price types" access permission ||
|| `400` | `100` | Could not find value for parameter {fields} | The required parameter `fields` is not provided ||
|| `400` | `0` | Required fields: name | The `name` field is not provided in `fields` ||
|| `400` | `ERROR_CORE` | The price type ID is missing.`<br>` | An empty string is passed in `name` ||
|| `400` | `0` | Error adding price type | Failed to save the price type ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-type-update.md)
- [{#T}](./catalog-price-type-get.md)
- [{#T}](./catalog-price-type-list.md)
- [{#T}](./catalog-price-type-delete.md)
- [{#T}](./catalog-price-type-get-fields.md)
