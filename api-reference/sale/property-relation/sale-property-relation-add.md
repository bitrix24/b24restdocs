# Add Property Binding sale.propertyRelation.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.propertyRelation.add` binds an order property to a payment system, delivery service, landing page, or trading platform.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **fields*** 
[`object`](../../data-types.md) | Fields of the new binding [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **entityId*** 
[`integer`](../../data-types.md) | Identifier of the binding object. Where to get it depends on `entityType`.

The method does not check whether the object exists. A non-numeric value is saved as `null` ||
|| **entityType*** 
[`string`](../../data-types.md) | Type of the binding object:
- `P` — payment system, identifier from [sale.paysystem.list](../../pay-system/sale-pay-system-list.md)
- `D` — delivery service, identifier from [sale.delivery.getlist](../delivery/delivery/sale-delivery-get-list.md)
- `L` — landing page, identifier from [landing.landing.getList](../../landing/page/methods/landing-landing-get-list.md)
- `T` — trading platform, identifier from [sale.tradePlatform.list](../trade-platform/sale-trade-platform-list.md)

The method does not validate the value: any character is saved, only the first character of a longer string is saved, and the response contains an empty array `[]` ||
|| **propertyId*** 
[`sale_order_property.id`](../data-types.md#sale_order_property) | Order property identifier. You can get the identifier using the [sale.property.list](../property/sale-property-list.md) method ||
|#

The combination of `entityType`, `entityId`, and `propertyId` must be unique. A binding cannot be modified: delete it using the [sale.propertyRelation.deleteByFilter](./sale-property-relation-delete-by-filter.md) method and create a new one.

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"entityId":6,"entityType":"D","propertyId":40}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertyRelation.add
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"entityId":6,"entityType":"D","propertyId":40},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertyRelation.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PropertyRelationAddResult = {
      propertyRelation: {
        entityId: number
        entityType: string
        propertyId: number
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<PropertyRelationAddResult>({
        method: 'sale.propertyRelation.add',
        params: {
          fields: {
            entityId: 6,
            entityType: 'D',
            propertyId: 40,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.propertyRelation)
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
      async function addPropertyRelation() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.propertyRelation.add',
            params: {
              fields: {
                entityId: 6,
                entityType: 'D',
                propertyId: 40,
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
          console.info(result.propertyRelation)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addPropertyRelation)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "entityId": 6,
        "entityType": "D",
        "propertyId": 40,
    }

    try:
        bitrix_response = client.sale.propertyrelation.add(
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
                'sale.propertyRelation.add',
                [
                    'fields' => [
                        'entityId'    => 6,
                        'entityType'  => 'D',
                        'propertyId'  => 40,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.propertyRelation.add',
        {
            fields: {
                entityId: 6,
                entityType: 'D',
                propertyId: 40
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.propertyRelation.add',
        [
            'fields' => [
                'entityId' => 6,
                'entityType' => 'D',
                'propertyId' => 40
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
    res, err := client.Core().Call(ctx, "sale.propertyRelation.add", b24.Params{
    	"fields": b24.Params{
    		"entityId":   6,
    		"entityType": "D",
    		"propertyId": 40,
    	},
    })
    if err != nil {
    	return fmt.Errorf("sale.propertyRelation.add: %w", err)
    }

    // The method wraps the response in an object with the "propertyRelation" key.
    raw, ok := b24.Unwrap(res.Result, "propertyRelation")
    if !ok {
    	return fmt.Errorf("no propertyRelation key in the response")
    }

    var item struct {
    	EntityID   b24.ID `json:"entityId"`
    	EntityType string `json:"entityType"`
    	PropertyID b24.ID `json:"propertyId"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.EntityID, item.EntityType)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "propertyRelation": {
            "entityId": 6,
            "entityType": "D",
            "propertyId": 40
        }
    },
    "time": {
        "start": 1712244475.495277,
        "finish": 1712244476.402808,
        "duration": 0.9075310230255127,
        "processing": 0.08538603782653809,
        "date_start": "2024-04-04T16:27:55+01:00",
        "date_finish": "2024-04-04T16:27:56+01:00"
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response. Contains the `propertyRelation` key — the created binding [(detailed description)](#propertyrelation) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object propertyRelation {#propertyrelation}

#|
|| **Name**
`type` | **Description** ||
|| **entityId**
[`integer`](../../data-types.md) | Identifier of the binding object ||
|| **entityType**
[`string`](../../data-types.md) | Type of the binding object: `P`, `D`, `L`, or `T` ||
|| **propertyId**
[`integer`](../../data-types.md) | Order property identifier ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "201650000001",
    "error_description": "Duplicate entry for key [propertyId, entityId, entityType]"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `201650000001` | Duplicate entry for key [propertyId, entityId, entityType] | Such a binding already exists ||
|| `400` | `201650000002` | property id is not exists | The property with the specified `propertyId` is not found ||
|| `400` | `0` | Required fields: entityId, entityType | Required fields are not provided; the text lists the missing ones ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter is not provided ||
|| `400` | `200040300020` | Access Denied | No permission to modify data in the Online Store module ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-property-relation-list.md)
- [{#T}](./sale-property-relation-delete-by-filter.md)
- [{#T}](./sale-property-relation-get-fields.md)
- [{#T}](../../../tutorials/sale/delivery-in-crm.md)
