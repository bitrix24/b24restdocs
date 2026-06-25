# Delete CRM Item: crm.item.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with the "delete" permission for CRM object elements.

Deletes a CRM object element by its item ID and entity type ID.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`][1] | The identifier of the [system](../data-types.md#object_type) or [custom type](./user-defined-object-types/index.md) whose element we want to delete.

Numerical values for system types (Lead — 1, Deal — 2, Contact — 3, Company — 4, Invoice — 31, etc.) are listed in the [CRM object types reference](../data-types.md#object_type). The identifier of the SPA can be obtained using the [crm.type.list](./user-defined-object-types/crm-type-list.md) method. ||
|| **id***
[`integer`][1] | The identifier of the item to be deleted.

It can be obtained using the [`crm.item.list`](./crm-item-list.md) method or when creating an item with [`crm.item.add`](./crm-item-add.md). ||
|#


## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Deleting an item with `id = 1`, belonging to the SPA with `entityTypeId = 1268`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1268,"id":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1268,"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.delete
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<never[]>({
        method: 'crm.item.delete',
        params: {
          entityTypeId: 1268,
          id: 1,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Item deleted successfully', result)
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
      async function deleteItem() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.delete',
            params: {
              entityTypeId: 1268,
              id: 1,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Item deleted successfully', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteItem)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.item.delete',
                [
                    'entityTypeId' => 1268,
                    'id'           => 1,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            return;
        }
    
        echo 'Success: ' . print_r($result->data(), true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting item: ' . $e->getMessage();
    }
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.item.delete(
            entity_type_id=1268,
            bitrix_id=1,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- BX24.js

    ```js
        BX24.callMethod(
            'crm.item.delete',
            {
                entityTypeId: 1268,
                id: 1,
            },
            (result) => {
                if (result.error())
                {
                    console.error(result.error());

                    return;
                }

                console.info(result.data());
            },
        );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.delete',
        [
            'entityTypeId' => 1268,
            'id' => 1
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
    "result": [],
    "time": {
        "start": 1721657688.755373,
        "finish": 1721657689.65017,
        "duration": 0.8947970867156982,
        "processing": 0.6092040538787842,
        "date_start": "2024-07-22T16:14:48+02:00",
        "date_finish": "2024-07-22T16:14:49+02:00",
        "operating": 0
    }
}
```

### Returned Values

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`][1] | The root element of the response.

Returns an empty array `[]` in case of success. ||
|| **time**
[`time`][1] | Information about the request execution time ||
|#


## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Item not found"
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code**                          | **Description**                                     | **Value**                                                      ||
|| `allowed_only_intranet_user`     | Action is allowed only to intranet users | User is not an intranet user                   ||
|| `NOT_FOUND`                      | SPA not found                          | Occurs when an invalid `entityTypeId` is passed                ||
|| `NOT_FOUND`                      | Item not found                                | An item with the given `id` of type `entityTypeId` does not exist.       ||
|| `ACCESS_DENIED`                  | Access denied                                  | User does not have permission to delete items of type `entityTypeId`. ||
|#

{% include [System errors](./../../../_includes/system-errors.md) %}


## Continue Learning

- [{#T}](crm-item-add.md)
- [{#T}](crm-item-update.md)
- [{#T}](crm-item-get.md)
- [{#T}](crm-item-list.md)
- [{#T}](crm-item-fields.md)
- [{#T}](./object-fields.md)

[1]: ../../data-types.md