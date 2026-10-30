# Update Property Variant Fields sale.propertyvariant.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.propertyvariant.update` changes the name, code, display order, and description of an order property value variant.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_property_variant.id`](../data-types.md) | Variant identifier. You can retrieve it using the [sale.propertyvariant.list](./sale-property-variant-list.md) method ||
|| **fields***
[`object`](../../data-types.md) | Variant fields [(detailed description)](#fields).

Pass the `name` and `value` fields in every call, even if you change only `sort` or `description`. If `sort` and `description` are not passed, they retain their current values ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Variant name. Pass the current name if you are not changing it ||
|| **value***
[`string`](../../data-types.md) | Symbolic code of the variant. Pass the current code if you are not changing it. You can retrieve the current values using the [sale.propertyvariant.get](./sale-property-variant-get.md) method ||
|| **sort**
[`integer`](../../data-types.md) | Display order of the variant in the list ||
|| **description**
[`string`](../../data-types.md) | Variant description. To clear the description, pass an empty string ||
|#

You cannot move a variant to another property: if you pass `orderPropsId`, the method ignores it without an error.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":5,"fields":{"name":"Red","value":"red","sort":10,"description":"New description for the red color value"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertyvariant.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":5,"fields":{"name":"Red","value":"red","sort":10,"description":"New description for the red color value"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertyvariant.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PropertyVariantUpdateResult = {
      propertyVariant: {
        description: string
        id: number
        name: string
        orderPropsId: number
        sort: number
        value: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<PropertyVariantUpdateResult>({
        method: 'sale.propertyvariant.update',
        params: {
          id: 5,
          fields: {
            name: 'Red',
            value: 'red',
            sort: 10,
            description: 'New description for the red color value',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.propertyVariant)
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
      async function updatePropertyVariant() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.propertyvariant.update',
            params: {
              id: 5,
              fields: {
                name: 'Red',
                value: 'red',
                sort: 10,
                description: 'New description for the red color value',
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
          console.info(result.propertyVariant)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updatePropertyVariant)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "name": "Red",
        "value": "red",
        "sort": 10,
        "description": "New description of the value for the red color",
    }

    try:
        bitrix_response = client.sale.propertyvariant.update(
            bitrix_id=5,
            fields=fields,
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
                'sale.propertyvariant.update',
                [
                    'id' => 5,
                    'fields' => [
                        'name'        => 'Red',
                        'value'       => 'red',
                        'sort'        => 10,
                        'description' => 'New description for the red color value',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating property variant: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.propertyvariant.update", {
            "id": 5,
            "fields": {
                "name": "Red",
                "value": "red",
                "sort": 10,
                "description": "New description for the red color value"
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.propertyvariant.update',
        [
            'id' => 5,
            'fields' => [
                'name' => 'Red',
                'value' => 'red',
                'sort' => 10,
                'description' => 'New description for the red color value'
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
    res, err := client.Core().Call(ctx, "sale.propertyvariant.update", b24.Params{
    	"id": 5,
    	"fields": b24.Params{
    		"name":        "Red",
    		"value":       "red",
    		"sort":        10,
    		"description": "New description for the red color value",
    	},
    })
    if err != nil {
    	return fmt.Errorf("sale.propertyvariant.update: %w", err)
    }

    // The method wraps the response in an object with the "propertyVariant" key.
    raw, ok := b24.Unwrap(res.Result, "propertyVariant")
    if !ok {
    	return fmt.Errorf("no propertyVariant key in the response")
    }

    var item struct {
    	Description  string `json:"description"`
    	ID           b24.ID `json:"id"`
    	Name         string `json:"name"`
    	OrderPropsID b24.ID `json:"orderPropsId"`
    	Sort         int    `json:"sort"`
    	Value        string `json:"value"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Description, item.ID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "propertyVariant": {
            "description": "New description for the red color value",
            "id": 5,
            "name": "Red",
            "orderPropsId": 49,
            "sort": 10,
            "value": "red"
        }
    },
    "time": {
        "start": 1711630589.257634,
        "finish": 1711630589.527446,
        "duration": 0.26981210708618164,
        "processing": 0.010741949081420898,
        "date_start": "2024-03-28T15:56:29+02:00",
        "date_finish": "2024-03-28T15:56:29+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **propertyVariant**
[`sale_order_property_variant`](#sale_order_property_variant) | Variant data [(detailed description)](#sale_order_property_variant) ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

#### sale_order_property_variant {#sale_order_property_variant}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Variant identifier ||
|| **orderPropsId**
[`integer`](../../data-types.md) | Identifier of the order property the variant belongs to ||
|| **name**
[`string`](../../data-types.md) | Variant name displayed to the customer ||
|| **value**
[`string`](../../data-types.md) | Symbolic code of the variant. Stored in the order as the property value ||
|| **sort**
[`integer`](../../data-types.md) | Display order of the variant in the list ||
|| **description**
[`string`](../../data-types.md) | Variant description. `null` if no description is set ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Required fields: value"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Required fields: `list of fields` | `fields` does not contain `name` or `value`. Both fields are required in every call ||
|| `400` | `201540400001` | property variant is not exists | The variant with the specified `id` was not found ||
|| `400` | `ERROR_NO_VALUE` | The property value variant is not specified | An empty string is passed in `value` ||
|| `400` | `ERROR_NO_NAME` | Name of the property value variant is not specified | An empty string is passed in `name` ||
|| `400` | `200040300020` | Access Denied | The user does not have permission to change store settings ||
|| `400` | `100` | Could not find value for parameter {`parameter name`} | The `id` or `fields` parameter is not provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-variant-add.md)
- [{#T}](./sale-property-variant-get.md)
- [{#T}](./sale-property-variant-list.md)
- [{#T}](./sale-property-variant-delete.md)
- [{#T}](./sale-property-variant-get-fields.md)
