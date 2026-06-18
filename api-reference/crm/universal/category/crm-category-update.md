# Update Sales Funnel crm.category.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with administrative access to the CRM section

This method updates the funnel (direction) with the identifier `id`, setting new values for the fields from `fields`. If any field is missing in `fields`, its value will remain unchanged.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`][1] | Identifier of the [system](../../index.md) or [user-defined type](../user-defined-object-types/index.md) of CRM entities for which the funnel will be updated            ||
|| **id***
[`integer`][1] | Identifier of the funnel. It can be obtained using the [`crm.category.list`](./crm-category-list.md) method or when creating a funnel with the [`crm.category.add`](./crm-category-add.md) method ||
|| **fields***
[`object`][1]  |  Field values (detailed description provided [below](#parametr-fields)) for updating the funnel fields in the form of a structure:

```js
fields: {
    name: "value",
    sort: "value",
    isDefault: "value",
},
```

  ||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`][1] | Name of the funnel. The name can be:
- length cannot exceed `255` characters
- cannot be empty
- cannot consist solely of spaces, tabs, etc.

Defaults to `-` ||
|| **sort**
[`integer`][1] | Sort index. 

Cannot be negative. If a value less than zero is passed to `sort`, it will be ignored and `sort = 0` will be set.

Defaults to `500` || 
|| **isDefault**
[`boolean`][1] | Indicates whether the funnel is the default funnel. Can have values:
- `Y` — yes, it is
- `N` — no

Defaults to `N`

In deals, the `isDefault` field is not available for modification.

You can find out if the field is immutable using the [`crm.category.fields`](./crm-category-fields.md) method. Immutable fields have the property `isReadonly = true` ||
|#

## Code Examples

How to update a funnel with `id = 4` in an SPA with `entityTypeId = 1152`

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1152,"id":4,"fields":{"name":"New Funnel Name","sort":1000,"isDefault":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.category.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1152,"id":4,"fields":{"name":"New Funnel Name","sort":1000,"isDefault":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.category.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CategoryUpdateResult = {
      category: {
        id: number
        name: string
        sort: number
        entityTypeId: number
        isDefault: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<CategoryUpdateResult>({
        method: 'crm.category.update',
        params: {
          entityTypeId: 1152,
          id: 4,
          fields: {
            name: 'New funnel name',
            sort: 1000,
            isDefault: 'Y',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.category.id, result.category.name)
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
      async function updateCategory() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.category.update',
            params: {
              entityTypeId: 1152,
              id: 4,
              fields: {
                name: 'New funnel name',
                sort: 1000,
                isDefault: 'Y',
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
          console.info(result.category.id, result.category.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateCategory)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.category.update',
                [
                    'entityTypeId' => 1152,
                    'id'          => 4,
                    'fields'      => [
                        'name'     => 'New Funnel Name',
                        'sort'     => 1000,
                        'isDefault' => 'Y',
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
        echo 'Error updating category: ' . $e->getMessage();
    }
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.category.update(
            bitrix_id=4,
            fields={
                "name": "New pipeline name",
                "sort": 1000,
                "isDefault": "Y",
            },
            entity_type_id=1152,
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

- BX24.js

    ```js
    BX24.callMethod(
        "crm.category.update",
        {
            entityTypeId: 1152,
            id: 4,
            fields: {
                name: "New Funnel Name",
                sort: 1000,
                isDefault: "Y",
            },
        },
        (result) => 
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.category.update',
        [
            'entityTypeId' => 1152,
            'id' => 4,
            'fields' => [
                'name' => "New Funnel Name",
                'sort' => 1000,
                'isDefault' => "Y"
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
        "category": {
            "id": 4,
            "name": "New Funnel Name",
            "sort": 1000,
            "entityTypeId": 1152,
            "isDefault": "Y"
        }
    },
    "time": {
        "start": 1718359296.368324,
        "finish": 1718359296.65352,
        "duration": 0.28519606590270996,
        "processing": 0.03645014762878418,
        "date_start": "2024-06-14T12:01:36+02:00",
        "date_finish": "2024-06-14T12:01:36+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response. Contains the [`category`](./crm-category-add.md#category) object ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Smart process not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `NOT_FOUND` | Smart process not found | Occurs with incorrect values for `entityTypeId` ||
|| `ENTITY_TYPE_NOT_SUPPORTED` | Entity type `{entityTypeName}` is not supported | Occurs if the CRM object does not support funnels ||
|| `NOT_FOUND` | Element not found | Occurs if the funnel being updated does not exist ||
|| `ACCESS_DENIED` | Access denied | Occurs if the user updating the funnel does not have sufficient rights ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-category-add.md)
- [{#T}](./crm-category-get.md)
- [{#T}](./crm-category-list.md)
- [{#T}](./crm-category-delete.md)
- [{#T}](./crm-category-fields.md)

[1]: ../../../data-types.md