# Delete Product Row from CRM Object crm.item.productrow.delete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: access permission to modify the CRM object that the product row belongs to is required.

Removes a product row from a CRM object.

After the deletion, CRM recalculates the total of the object: for example, the deal total decreases by the cost of the deleted row. A deleted row cannot be restored — it has to be added again with the [crm.item.productrow.add](./crm-item-productrow-add.md) method.

If the row has already been included in a payment, the method also deletes the corresponding [product item in the payment](../payment/products-in-payment/index.md).

The method deletes a single row. To delete several rows at once, pass the required set to the [crm.item.productrow.set](./crm-item-productrow-set.md) method: it replaces all product rows of the CRM object.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`crm_item_product_row.id`](../../data-types.md#crm_item_product_row) | Identifier of the product row.
It can be retrieved with the [crm.item.productrow.list](./crm-item-productrow-list.md) method. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":17655}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.productrow.delete
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":17655,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.productrow.delete
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<null>({
        method: 'crm.item.productrow.delete',
        params: {
          id: 17655,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Product row deleted, result:', result)
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
      async function deleteProductRow() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.productrow.delete',
            params: {
              id: 17655,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Product row deleted, result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteProductRow)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.item.productrow.delete(
            bitrix_id=17655,
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

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.item.productrow.delete',
                [
                    'id' => 17655,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting product row: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.productrow.delete', {
            id: 17655,
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
        'crm.item.productrow.delete',
        [
            'id' => 17655
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    _, err := client.Core().Call(ctx, "crm.item.productrow.delete", b24.Params{
    	"id": 17655,
    })
    if err != nil {
    	return fmt.Errorf("crm.item.productrow.delete: %w", err)
    }

    // The method returns null — there is nothing to unmarshal, it is enough to check that there is no error.
    fmt.Println("product row deleted")
    ```

{% endlist %}

## Response on Success

HTTP status: **200**

```json
{
   "result":null,
   "time":{
      "start":1716898012.405785,
      "finish":1716898013.536588,
      "duration":1.130802869796753,
      "processing":0.9562788009643555,
      "date_start":"2024-05-28T15:06:52+03:00",
      "date_finish":"2024-05-28T15:06:53+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`null`](../../../data-types.md) | The method returns no data. Success is indicated by the absence of an error in the response ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":"NOT_FOUND",
   "error_description":"Item not found"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ENTITY_TYPE_NOT_SUPPORTED` | This type of CRM object does not support product rows ||
|| `ACCESS_DENIED` | The user has no permission to modify the CRM object that the product row belongs to ||
|| `NOT_FOUND` | Product row not found. The same error is returned when an already deleted row is deleted again ||
|| `100` | Required parameters not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-productrow-add.md)
- [{#T}](./crm-item-productrow-update.md)
- [{#T}](./crm-item-productrow-get.md)
- [{#T}](./crm-item-productrow-list.md)
- [{#T}](./crm-item-productrow-set.md)
- [{#T}](./crm-item-productrow-get-available-for-payment.md)
- [{#T}](./crm-item-productrow-fields.md)
