# Update Property Variant Fields sale.propertyvariant.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the value variant of a property. It is applicable only for properties of type `ENUM`.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_property_variant.id`](../data-types.md) | Identifier of the property value variant ||
|| **fields***
[`object`](../../data-types.md) | Field values to update the property value variant ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Name of the property value variant ||
|| **value***
[`string`](../../data-types.md) | Value (code) of the property value variant ||
|| **sort**
[`integer`](../../data-types.md) | Sorting ||
|| **description**
[`string`](../../data-types.md) | Description of the property value variant ||
|#

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
[`sale_order_property_variant`](../data-types.md) | Object with information about the updated property value variant ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
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
|| `201540400001` | The updated property value variant was not found ||
|| `200040300020` | Insufficient permissions to update the property value variant ||
|| `100` | Parameter `id` is not specified ||
|| `100` | Parameter `fields` is not specified or is empty ||
|| `0` | Required fields in the `fields` structure are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|| `ERROR_NO_VALUE` | An empty value for the property value variant's character code was provided ||
|| `ERROR_NO_NAME` | An empty value for the property value variant's name was provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-variant-add.md)
- [{#T}](./sale-property-variant-get.md)
- [{#T}](./sale-property-variant-list.md)
- [{#T}](./sale-property-variant-delete.md)
- [{#T}](./sale-property-variant-get-fields.md)
