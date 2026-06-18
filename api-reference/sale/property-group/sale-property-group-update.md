# Update Property Group Fields sale.propertygroup.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the fields of a property group.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_property_group.id`](../data-types.md) | Identifier of the property group ||
|| **fields***
[`object`](../../data-types.md) | Field values for updating the property group ||
|#

## fields Parameter

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Parameter** / **Type** | **Description** ||
|| **personTypeId***
[`sale_person_type.id`](../data-types.md) | Identifier of the payer type. Since the property is marked with the `isImmutable` indicator, it cannot be changed. The value of the property must be provided ||
|| **name***
[`string`](../../data-types.md) | Name of the property group ||
|| **sort**
[`integer`](../../data-types.md) | Sorting ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":10,"fields":{"personTypeId":3,"name":"Updated Property Group","sort":100}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertygroup.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":10,"fields":{"personTypeId":3,"name":"Updated Property Group","sort":100},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertygroup.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PropertyGroupUpdateResult = {
      propertyGroup: {
        id: number
        name: string
        personTypeId: number
        sort: number
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<PropertyGroupUpdateResult>({
        method: 'sale.propertygroup.update',
        params: {
          id: 10,
          fields: {
            personTypeId: 3,
            name: 'Updated property group',
            sort: 100,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.propertyGroup)
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
      async function updatePropertyGroup() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.propertygroup.update',
            params: {
              id: 10,
              fields: {
                personTypeId: 3,
                name: 'Updated property group',
                sort: 100,
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
          console.info(result.propertyGroup)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updatePropertyGroup)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.propertygroup.update',
                [
                    'id' => 10,
                    'fields' => [
                        'personTypeId' => 3,
                        'name' => 'Updated Property Group',
                        'sort' => 100,
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
        echo 'Error updating property group: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.propertygroup.update", {
            "id": 10,
            "fields": {
                "personTypeId": 3,
                "name": "Updated Property Group",
                "sort": 100,
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
        'sale.propertygroup.update',
        [
            'id' => 10,
            'fields' => [
                'personTypeId' => 3,
                'name' => 'Updated Property Group',
                'sort' => 100,
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "propertyGroup": {
            "id": 10,
            "name": "Updated Property Group",
            "personTypeId": 3,
            "sort": 100
        }
    },
    "time": {
        "start": 1711451989.729911,
        "finish": 1711451989.907491,
        "duration": 0.1775798797607422,
        "processing": 0.008534908294677734,
        "date_start": "2024-03-26T14:19:49+02:00",
        "date_finish": "2024-03-26T14:19:49+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **propertyGroup**
[`sale_order_property_group`](../data-types.md) | Object with information about the updated property group ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

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
|| `200940400001` | The property group to be updated was not found ||
|| `200040300020` | Insufficient permissions to update the property group ||
|| `200950000008` | An empty value was provided for one of the required fields ||
|| `100` | The `id` parameter is not specified ||
|| `100` | The `fields` parameter is not specified or is empty ||
|| `0` | Required fields of the `fields` structure were not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-group-add.md)
- [{#T}](./sale-property-group-get.md)
- [{#T}](./sale-property-group-list.md)
- [{#T}](./sale-property-group-delete.md)
- [{#T}](./sale-property-group-get-fields.md)
