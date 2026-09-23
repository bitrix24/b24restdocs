# Get the List of Fields for Product Rows crm.item.productrow.fields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

Retrieves the description of product row fields: the value type, whether the field is required, and whether it is available for writing.

Call the method before [crm.item.productrow.add](./crm-item-productrow-add.md), [crm.item.productrow.update](./crm-item-productrow-update.md), or [crm.item.productrow.set](./crm-item-productrow-set.md) to find out which values can be passed. The method calculates the fields with `isReadOnly: true` on its own and ignores the values passed for them without raising an error.

The set of fields is the same for all types of CRM objects and does not depend on `ownerType`. The limits on the length of text fields are described in the [{#T}](../../field-length-limits.md) article.

The `type` value is the data type of the field from the [type dictionary](../../../data-types.md). Fields of the `char` type accept only `Y` or `N`.

The `isRequired` flag reflects the field description in the core and does not always match the checks of a write method. For example, `productId` comes with `isRequired: true`, but a product row can be created without it — passing `productName` is enough.

## Method Parameters

No parameters.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.productrow.fields
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.productrow.fields
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldItem = {
      type: string
      isRequired: boolean
      isReadOnly: boolean
      isImmutable: boolean
      isMultiple: boolean
      isDynamic: boolean
      title: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ProductRowFieldsResult = {
      fields: Record<string, FieldItem>
    }

    try {
      const response = await $b24.actions.v2.call.make<ProductRowFieldsResult>({
        method: 'crm.item.productrow.fields',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(Object.keys(result.fields))
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
      async function getProductRowFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.productrow.fields',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(Object.keys(result.fields))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getProductRowFields)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.item.productrow.fields().response
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
                'crm.item.productrow.fields',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching product row fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.productrow.fields', {},
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
        'crm.item.productrow.fields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.item.productrow.fields", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.item.productrow.fields: %w", err)
    }

    // The method wraps the response in an object with the "fields" key.
    raw, ok := b24.Unwrap(res.Result, "fields")
    if !ok {
    	return fmt.Errorf("no fields key in the response")
    }

    fmt.Printf("%s\n", raw)
    ```

{% endlist %}

## Response on Success

HTTP status: **200**

```json
{
   "result":{
      "fields":{
         "id":{
            "type":"integer",
            "isRequired":false,
            "isReadOnly":true,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"ID"
         },
         "ownerId":{
            "type":"integer",
            "isRequired":true,
            "isReadOnly":false,
            "isImmutable":true,
            "isMultiple":false,
            "isDynamic":false,
            "title":"Owner ID"
         },
         "ownerType":{
            "type":"string",
            "isRequired":true,
            "isReadOnly":false,
            "isImmutable":true,
            "isMultiple":false,
            "isDynamic":false,
            "title":"Owner Type"
         },
         "productId":{
            "type":"integer",
            "isRequired":true,
            "isReadOnly":false,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"Product"
         },
         "productName":{
            "type":"string",
            "isRequired":false,
            "isReadOnly":false,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"Product Name"
         },
         "price":{
            "type":"double",
            "isRequired":false,
            "isReadOnly":false,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"Price"
         },
         "priceExclusive":{
            "type":"double",
            "isRequired":false,
            "isReadOnly":true,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"Discounted price excluding tax"
         },
         "priceNetto":{
            "type":"double",
            "isRequired":false,
            "isReadOnly":true,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"PRICE_NETTO"
         },
         "priceBrutto":{
            "type":"double",
            "isRequired":false,
            "isReadOnly":true,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"PRICE_BRUTTO"
         },
         "quantity":{
            "type":"double",
            "isRequired":false,
            "isReadOnly":false,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"Quantity"
         },
         "discountTypeId":{
            "type":"integer",
            "isRequired":false,
            "isReadOnly":false,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"Discount Type"
         },
         "discountRate":{
            "type":"double",
            "isRequired":false,
            "isReadOnly":false,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"Discount Amount"
         },
         "discountSum":{
            "type":"double",
            "isRequired":false,
            "isReadOnly":false,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"Discount Total"
         },
         "taxRate":{
            "type":"double",
            "isRequired":false,
            "isReadOnly":false,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"Tax"
         },
         "taxName":{
            "type":"string",
            "isRequired":false,
            "isReadOnly":false,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"TAX_NAME"
         },
         "taxIncluded":{
            "type":"char",
            "isRequired":false,
            "isReadOnly":false,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"Tax included in price"
         },
         "customized":{
            "type":"char",
            "isRequired":false,
            "isReadOnly":false,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"Modified"
         },
         "measureCode":{
            "type":"integer",
            "isRequired":false,
            "isReadOnly":false,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"Unit of Measure Code"
         },
         "measureName":{
            "type":"string",
            "isRequired":false,
            "isReadOnly":false,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"Unit of Measure"
         },
         "sort":{
            "type":"integer",
            "isRequired":false,
            "isReadOnly":false,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"Sorting"
         },
         "type":{
            "type":"integer",
            "isRequired":false,
            "isReadOnly":true,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"TYPE"
         },
         "storeId":{
            "type":"integer",
            "isRequired":false,
            "isReadOnly":true,
            "isImmutable":false,
            "isMultiple":false,
            "isDynamic":false,
            "title":"STORE_ID"
         }
      }
   },
   "time":{
      "start":1716812240.400023,
      "finish":1716812242.151703,
      "duration":1.7516798973083496,
      "processing":0.09682416915893555,
      "date_start":"2024-05-27T15:17:20+03:00",
      "date_finish":"2024-05-27T15:17:22+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### The result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **fields**
[`object`](../../../data-types.md) | Object in the format `{"field code": "field description"}`, where the key is the identifier of a field of the [crm_item_product_row](../../data-types.md#crm_item_product_row) object, and the value is an object of type [crm_rest_field_description](../../data-types.md#crm_rest_field_description) ||
|#

What the response does not contain: the `priceAccount` and `xmlId` fields, which cannot be written, and the `upperName` flag of the [crm_rest_field_description](../../data-types.md#crm_rest_field_description) object. The full composition of a product row is described in the [crm_item_product_row](../../data-types.md#crm_item_product_row) object.

The `measureName` and `customized` fields come with the `isReadOnly: false` flag, but write methods do not retain their values. The `customized` field is also deprecated — there is no need to pass it.

## Error Handling

The method has no error codes of its own: it accepts no parameters and does not check permissions for CRM objects. Only system errors are returned — for example, `insufficient_scope` if the webhook or the application has no `crm` permission.

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

{% include notitle [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-productrow-add.md)
- [{#T}](./crm-item-productrow-update.md)
- [{#T}](./crm-item-productrow-get.md)
- [{#T}](./crm-item-productrow-list.md)
- [{#T}](./crm-item-productrow-delete.md)
- [{#T}](./crm-item-productrow-set.md)
- [{#T}](./crm-item-productrow-get-available-for-payment.md)
