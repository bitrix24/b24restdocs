# Add Property Variant sale.propertyvariant.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a variant value for a property. It is applicable only for properties of type `ENUM`.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating a property variant ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **orderPropsId***
[`sale_order_property.id`](../data-types.md) | Property identifier ||
|| **name***
[`string`](../../data-types.md) | Name of the property variant value ||
|| **value***
[`string`](../../data-types.md) | Symbolic code of the property variant value ||
|| **sort**
[`integer`](../../data-types.md) | Sorting ||
|| **description**
[`string`](../../data-types.md) | Description of the property variant value ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Red","orderPropsId":49,"value":"red","sort":10,"description":"Description of the value for red color"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertyvariant.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Red","orderPropsId":49,"value":"red","sort":10,"description":"Description of the value for red color"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertyvariant.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PropertyVariantAddResult = {
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
      const response = await $b24.actions.v2.call.make<PropertyVariantAddResult>({
        method: 'sale.propertyvariant.add',
        params: {
          fields: {
            name: 'Red',
            orderPropsId: 49,
            value: 'red',
            sort: 10,
            description: 'Description of the value for the red color',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.propertyVariant.id, result.propertyVariant.name, result.propertyVariant.value)
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
      async function addPropertyVariant() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.propertyvariant.add',
            params: {
              fields: {
                name: 'Red',
                orderPropsId: 49,
                value: 'red',
                sort: 10,
                description: 'Description of the value for the red color',
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
          console.info(result.propertyVariant.id, result.propertyVariant.name, result.propertyVariant.value)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addPropertyVariant)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.propertyvariant.add',
                [
                    'fields' => [
                        'name'         => 'Red',
                        'orderPropsId' => 49,
                        'value'        => 'red',
                        'sort'         => 10,
                        'description'  => 'Description of the value for red color',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding property variant: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.propertyvariant.add", {
            "fields": {
                "name": "Red",
                "orderPropsId": 49,
                "value": "red",
                "sort": 10,
                "description": "Description of the value for red color"
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
        'sale.propertyvariant.add',
        [
            'fields' => [
                'name' => 'Red',
                'orderPropsId' => 49,
                'value' => 'red',
                'sort' => 10,
                'description' => 'Description of the value for red color'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "propertyVariant": {
            "description": "Description of the value for red color",
            "id": 5,
            "name": "Red",
            "orderPropsId": 49,
            "sort": 10,
            "value": "red"
        }
    },
    "time": {
        "start": 1711629310.006284,
        "finish": 1711629310.334167,
        "duration": 0.3278830051422119,
        "processing": 0.024754047393798828,
        "date_start": "2024-03-28T15:35:10+02:00",
        "date_finish": "2024-03-28T15:35:10+02:00"
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
[`sale_order_property_variant`](../data-types.md) | Object with information about the added property variant value ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 0,
    "error_description": "Required fields: name"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201550000003` | Property not found for which the property variant value is being added ||
|| `200040300020` | Insufficient permissions to add the property variant value ||
|| `100` | Parameter `fields` is not specified or is empty ||
|| `0` | Required fields of the `fields` structure are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|| `ERROR_NO_VALUE` | Empty value for the symbolic code of the property variant value provided ||
|| `ERROR_NO_NAME` | Empty value for the name of the property variant value provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-variant-update.md)
- [{#T}](./sale-property-variant-get.md)
- [{#T}](./sale-property-variant-list.md)
- [{#T}](./sale-property-variant-delete.md)
- [{#T}](./sale-property-variant-get-fields.md)
