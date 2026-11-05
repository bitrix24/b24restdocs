# Update the Value of the List Property catalog.productPropertyEnum.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View Product Catalog" permission and the right to modify the information block property.

The method `catalog.productPropertyEnum.update` updates the value of a list property.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the list property value.

The identifier can be obtained using the [catalog.productPropertyEnum.list](./catalog-product-property-enum-list.md) method. ||
|| **fields***
[`object`](../../data-types.md) | Object with the fields of the list value [(detailed description)](#fields).

The `propertyId`, `value`, and `xmlId` fields are required on every call: to keep a field unchanged, pass its current value from [catalog.productPropertyEnum.get](./catalog-product-property-enum-get.md). The optional `def` and `sort` fields retain their current values if you do not pass them ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **propertyId***
[`catalog_product_property.id`](../data-types.md#catalog_product_property) | Identifier of the product or variation property.

Property identifiers can be obtained using the [catalog.productProperty.list](../product-property/catalog-product-property-list.md) method.

You cannot move a value to another property: if you pass the `propertyId` of another property, the method skips it without an error and the value stays with its current property ||
|| **value***
[`string`](../../data-types.md) | Value of the list item. ||
|| **xmlId***
[`string`](../../data-types.md) | External identifier of the list value. Must be unique within the property. ||
|| **def**
[`char`](../../data-types.md) | Indicator of the default value. Acceptable values:
- `Y` — default
- `N` — not default

Any other value causes the `BX_INVALID_VALUE` error ||
|| **sort**
[`integer`](../../data-types.md) | Sort index. ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"id":1739,"fields":{"propertyId":431,"value":"Medium","xmlId":"M","def":"N","sort":110}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productPropertyEnum.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"id":1739,"fields":{"propertyId":431,"value":"Medium","xmlId":"M","def":"N","sort":110},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/catalog.productPropertyEnum.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ProductPropertyEnumUpdateResult = {
      productPropertyEnum: {
        def: string,
        id: number,
        propertyId: number,
        sort: number,
        value: string,
        xmlId: string,
      },
    }

    try {
      const response = await $b24.actions.v2.call.make<ProductPropertyEnumUpdateResult>({
        method: 'catalog.productPropertyEnum.update',
        params: {
          id: 1739,
          fields: {
            propertyId: 431,
            value: 'Medium',
            xmlId: 'M',
            def: 'N',
            sort: 110,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.productPropertyEnum)
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
      async function updateProductPropertyEnum() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.productPropertyEnum.update',
            params: {
              id: 1739,
              fields: {
                propertyId: 431,
                value: 'Medium',
                xmlId: 'M',
                def: 'N',
                sort: 110,
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
          console.info(result.productPropertyEnum)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateProductPropertyEnum)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product_property_enum.update(
            bitrix_id=1739,
            fields={
                "propertyId": 431,
                "value": "Medium",
                "xmlId": "M",
                "sort": 110,
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
                'catalog.productPropertyEnum.update',
                [
                    'id' => 1739,
                    'fields' => [
                        'propertyId' => 431,
                        'value' => 'Medium',
                        'xmlId' => 'M',
                        'def' => 'N',
                        'sort' => 110,
                    ],
                ]
            );

        print_r($response->getResponseData()->getResult());
    } catch (\Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.productPropertyEnum.update',
        {
            id: 1739,
            fields: {
                propertyId: 431,
                value: 'Medium',
                xmlId: 'M',
                def: 'N',
                sort: 110,
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.productPropertyEnum.update',
        [
            'id' => 1739,
            'fields' => [
                'propertyId' => 431,
                'value' => 'Medium',
                'xmlId' => 'M',
                'def' => 'N',
                'sort' => 110,
            ]
        ]
    );

    print_r($result);
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.productPropertyEnum.update", b24.Params{
    	"id": 1739,
    	"fields": b24.Params{
    		"propertyId": 431,
    		"value":      "Medium",
    		"xmlId":      "M",
    		"def":        "N",
    		"sort":       110,
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.productPropertyEnum.update: %w", err)
    }

    // The method wraps the response in an object with the "productPropertyEnum" key.
    raw, ok := b24.Unwrap(res.Result, "productPropertyEnum")
    if !ok {
    	return fmt.Errorf("no productPropertyEnum key in the response")
    }

    var item struct {
    	Def        string `json:"def"`
    	ID         b24.ID `json:"id"`
    	PropertyID b24.ID `json:"propertyId"`
    	Sort       int    `json:"sort"`
    	Value      string `json:"value"`
    	XmlID      string `json:"xmlId"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Def, item.ID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "productPropertyEnum": {
        "def": "N",
        "id": 1739,
        "propertyId": 431,
        "sort": 110,
        "value": "Medium",
        "xmlId": "M"
        }
    },
    "time": {
        "start": 1774339029,
        "finish": 1774339030.119726,
        "duration": 1.1197259426116943,
        "processing": 1,
        "date_start": "2026-03-24T10:57:09+01:00",
        "date_finish": "2026-03-24T10:57:10+01:00",
        "operating_reset_at": 1774339629,
        "operating": 0.1804349422454834
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root object of the response. ||
|| **result.productPropertyEnum**
[`catalog_product_property_enum`](../data-types.md#catalog_product_property_enum) | Object of the updated list property value. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "productPropertyEnum does not exist."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Access Denied | The user does not have the "View product catalog" permission or permission to modify the information block of the property ||
|| `400` | `100` | Could not find value for parameter {id} | The required parameter `id` was not provided ||
|| `400` | `100` | Could not find value for parameter {fields} | The required parameter `fields` was not provided ||
|| `400` | `0` | Required fields: propertyId, value, xmlId | Required fields are missing in `fields`. The message lists only the missing fields ||
|| `400` | Empty value | productPropertyEnum does not exist. | The value with the provided `id` was not found, the property from `propertyId` was not found, or they do not belong to the product catalog ||
|| `400` | `0` | The specified property does not belong to a product catalog | The property does not belong to the product catalog ||
|| `400` | `BX_INVALID_VALUE` | A record "External ID" set to "..." already exists in the database. | The property already has a value with this `xmlId` ||
|| `400` | `BX_INVALID_VALUE` | The value of "Default" is invalid. | `def` contains a value other than `Y` and `N` ||
|| `400` | `BX_EMPTY_REQUIRED` | The field Value is required. | `value` contains an empty string ||
|| `400` | `0` | Internal error updating enumeration value. Try updating again. | An internal error occurred while updating the list value ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-enum-add.md)
- [{#T}](./catalog-product-property-enum-get.md)
- [{#T}](./catalog-product-property-enum-list.md)
- [{#T}](./catalog-product-property-enum-delete.md)
- [{#T}](./catalog-product-property-enum-get-fields.md)