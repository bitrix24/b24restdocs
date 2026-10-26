# Get Fields and Settings of Shipment Property for a Specific Property Type sale.shipmentproperty.getfieldsbytype

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.shipmentproperty.getfieldsbytype` retrieves the available fields of shipment properties by property type.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type***
[`string`](../../data-types.md) | Shipment property type.

Possible values:
- `STRING` — string
- `Y/N` — yes or no
- `NUMBER` — number
- `ENUM` — list
- `FILE` — file
- `DATE` — date
- `LOCATION` — location
- `ADDRESS` — address ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"NUMBER"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipmentproperty.getfieldsbytype
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"NUMBER","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipmentproperty.getfieldsbytype
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldInfo = {
      isImmutable: boolean
      isReadOnly: boolean
      isRequired: boolean
      type: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type GetFieldsByTypeResult = {
      property: Record<string, FieldInfo>
    }

    try {
      const response = await $b24.actions.v2.call.make<GetFieldsByTypeResult>({
        method: 'sale.shipmentproperty.getfieldsbytype',
        params: {
          type: 'NUMBER',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Available property fields:', Object.keys(result.property))
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
      async function getShipmentPropertyFieldsByType() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.shipmentproperty.getfieldsbytype',
            params: {
              type: 'NUMBER',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Available property fields:', Object.keys(result.property))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getShipmentPropertyFieldsByType)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.sale.shipmentproperty.getfieldsbytype(
            type='NUMBER',
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
                'sale.shipmentproperty.getfieldsbytype',
                [
                    'type' => 'NUMBER',
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
        echo 'Error getting shipment property fields by type: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.shipmentproperty.getfieldsbytype", {
            "type": "NUMBER",
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
        'sale.shipmentproperty.getfieldsbytype',
        [
            'type' => 'NUMBER'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "sale.shipmentproperty.getfieldsbytype", b24.Params{
    	"type": "NUMBER",
    })
    if err != nil {
    	return fmt.Errorf("sale.shipmentproperty.getfieldsbytype: %w", err)
    }

    // The method wraps the response in an object with the "property" key.
    raw, ok := b24.Unwrap(res.Result, "property")
    if !ok {
    	return fmt.Errorf("no property key in the response")
    }

    fmt.Printf("%s\n", raw)
    ```

{% endlist %}

## Successful Response

HTTP Status: **200**

```json
{
    "result": {
        "property": {
            "active": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "code": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "defaultValue": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "any"
            },
            "description": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            },
            "id": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "integer"
            },
            "isFiltered": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "multiple": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "name": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": true,
                "type": "string"
            },
            "personTypeId": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": true,
                "type": "integer"
            },
            "propsGroupId": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": true,
                "type": "integer"
            },
            "required": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "settings": {
                "fields": {
                    "max": {
                        "isImmutable": false,
                        "isReadOnly": false,
                        "isRequired": false,
                        "type": "integer"
                    },
                    "min": {
                        "isImmutable": false,
                        "isReadOnly": false,
                        "isRequired": false,
                        "type": "integer"
                    },
                    "step": {
                        "isImmutable": false,
                        "isReadOnly": false,
                        "isRequired": false,
                        "type": "integer"
                    }
                },
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "datatype"
            },
            "sort": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "integer"
            },
            "type": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": true,
                "type": "string"
            },
            "userProps": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
            },
            "util": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "char"
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
        "start": 1793012310,
        "finish": 1793012310.892729,
        "duration": 0.8927290439605713,
        "processing": 0,
        "date_start": "2026-10-26T12:58:30+02:00",
        "date_finish": "2026-10-26T12:58:30+02:00",
        "operating_reset_at": 1793012880,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **property**
[`object`](../../data-types.md) | Shipment property fields available for the specified type. The key is the field name, and the value is an object describing the field. The structure is described [below](#field)

The set of fields depends on the type:
- all types — common fields `id`, `personTypeId`, `propsGroupId`, `name`, `type`, `code`, `active`, `util`, `userProps`, `isFiltered`, `sort`, `description`, `required`, `multiple`, `defaultValue`, `settings`, `xmlId`
- `STRING` — additionally `isProfileName`, `isPayer`, `isEmail`, `isPhone`, `isZip`, `isAddress`
- `LOCATION` — additionally `isLocation`, `isLocation4tax`, `inputFieldLocation`
- `ADDRESS` — additionally `isAddressFrom`, `isAddressTo`

Field values are described in the [sale_order_property](../data-types.md#sale_order_property) type ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Field Description {#field}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../data-types.md) | Field value type: `integer`, `string`, `char`, or `any`. For the `defaultValue` field, it is `any`; for the `settings` field, it is `datatype` ||
|| **isRequired**
[`boolean`](../../data-types.md) | The field is required when creating a property ||
|| **isReadOnly**
[`boolean`](../../data-types.md) | The field is read-only: its value cannot be set when creating or updating a property ||
|| **isImmutable**
[`boolean`](../../data-types.md) | The field is set only when creating a property and cannot be changed afterwards ||
|| **fields**
[`object`](../../data-types.md) | Present only in the `settings` field. Settings available for the property type, in the same format: the key is the setting name, and the value is the field description.

The set of settings depends on the type:
- `STRING` — `minlength`, `maxlength`, `pattern`, `multiline`, `cols`, `rows`, `size`
- `NUMBER` — `min`, `max`, `step`
- `ENUM` — `multielement`, `size`
- `FILE` — `maxsize`, `accept`
- `DATE` — `time`
- `Y/N`, `LOCATION`, `ADDRESS` — no settings, `fields` is an empty array ||
|#

## Error Handling

HTTP Status: **400**

```json
{
   "error":"200850000008",
   "error_description":"type is out of range"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200850000008` | An invalid `type` value was passed ||
|| `200040300010` | Insufficient permissions to read available shipment property fields ||
|| `100` | The `type` parameter is not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-shipment-property-add.md)
- [{#T}](./sale-shipment-property-update.md)
- [{#T}](./sale-shipment-property-get.md)
- [{#T}](./sale-shipment-property-list.md)
- [{#T}](./sale-shipment-property-delete.md)
