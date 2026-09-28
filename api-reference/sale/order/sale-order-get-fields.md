# Get Order Fields sale.order.getFields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.order.getFields` retrieves the description of order fields: the type of each field, whether it is required when creating an order, and whether it can be changed. Use the response to check field names for `select` and `filter` in [sale.order.list](./sale-order-list.md) and the set of `fields` for [sale.order.add](./sale-order-add.md) and [sale.order.update](./sale-order-update.md).

No parameters.

## Code Examples

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.order.getfields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.order.getfields
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type OrderFieldInfo = {
      isImmutable: boolean
      isReadOnly: boolean
      isRequired: boolean
      type: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type OrderGetFieldsResult = {
      order: Record<string, OrderFieldInfo>
    }

    try {
      const response = await $b24.actions.v2.call.make<OrderGetFieldsResult>({
        method: 'sale.order.getfields',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Order fields:', Object.keys(result.order))
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
      async function getOrderFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.order.getfields',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Order fields:', Object.keys(result.order))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getOrderFields)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.sale.order.getfields().response
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
                'sale.order.getfields',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        // SDK throws an exception on API errors, so here the call has succeeded
        echo 'Fields: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting order fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.order.getfields", {},
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
        'sale.order.getfields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "sale.order.getfields", nil, b24.WithIdempotent())
    if err != nil {
        return fmt.Errorf("sale.order.getfields: %w", err)
    }

    // The method wraps the response in an object with the "order" key.
    raw, ok := b24.Unwrap(res.Result, "order")
    if !ok {
        return fmt.Errorf("no order key in the response")
    }

    fmt.Printf("%s\n", raw)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "order": {
            "accountNumber": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "string"
            },
            "additionalInfo": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "affiliateId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "canceled": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "comments": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "companyId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "currency": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": true,
                "type": "string"
            },
            "dateCanceled": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "datetime"
            },
            "dateInsert": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "datetime"
            },
            "dateLock": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "datetime"
            },
            "dateMarked": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "datetime"
            },
            "dateStatus": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "datetime"
            },
            "dateUpdate": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "datetime"
            },
            "deducted": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "char"
            },
            "discountValue": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "double"
            },
            "empCanceledId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "empMarkedId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "empStatusId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "externalOrder": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "id": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "integer"
            },
            "id1c": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "lid": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": true,
                "type": "string"
            },
            "lockedBy": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "marked": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "orderTopic": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "payed": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "char"
            },
            "personTypeId": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": true,
                "type": "integer"
            },
            "personTypeXmlId": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "string"
            },
            "price": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "double"
            },
            "reasonCanceled": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "reasonMarked": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "recountFlag": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "recurringId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "responsibleId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "statusId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "statusXmlId": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "string"
            },
            "taxValue": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "double"
            },
            "updated1c": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "userDescription": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "userId": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "version": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "integer"
            },
            "version1c": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "xmlId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            }
        }
    },
    "time": {
        "start": 1712825736.172681,
        "finish": 1712825736.838046,
        "duration": 0.6653649806976318,
        "processing": 0.03579592704772949,
        "date_start": "2024-04-11T11:55:36+02:00",
        "date_finish": "2024-04-11T11:55:36+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **order**
[`object`](../../data-types.md) | The key is the name of a [sale_order](../data-types.md#sale_order) object field, and the value is the field description [(detailed description)](#field-description) ||
|#

#### Field Description {#field-description}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../data-types.md) | Type of the field value: `integer`, `double`, `string`, `char`, `datetime`. A `char` value is passed as a string. Flag fields, such as `canceled` or `marked`, take the values `Y` or `N` ||
|| **isRequired**
[`boolean`](../../data-types.md) | `true` — the field is required when creating an order. The required fields are `lid`, `personTypeId`, and `currency` ||
|| **isReadOnly**
[`boolean`](../../data-types.md) | `true` — the value is set by Bitrix24, so you do not need to pass it to `sale.order.add` and `sale.order.update`. For example, `id`, `accountNumber`, `payed`, `dateUpdate` ||
|| **isImmutable**
[`boolean`](../../data-types.md) | `true` — the value is set only when the order is created. The `sale.order.update` method does not change such a field and does not return an error. These fields are `lid`, `personTypeId`, `currency`, and `userId` ||
|#

## Error Handling

HTTP Status: **401**

```json
{
    "error": "insufficient_scope",
    "error_description": "The request requires higher privileges than provided by the webhook token"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | `Access Denied` | Insufficient permissions to read order fields ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-order-add.md)
- [{#T}](./sale-order-update.md)
- [{#T}](./sale-order-get.md)
- [{#T}](./sale-order-list.md)
- [{#T}](./sale-order-delete.md)
