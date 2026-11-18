# Update Address for crm.address.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the contact, company, or lead that owns the address

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use the REST 3.0 methods: for addresses of contacts, companies, and leads, see the [Addresses](../../rest-v3/addresses/index.md) group, and for requisite addresses, see the [Requisites](../../rest-v3/requisites/index.md) group.

{% endnote %}

Updates an existing address of a requisite or lead. Does not create a new address.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | A set of fields to update the address [(detailed description)](#fields).

Always pass `TYPE_ID`, `ENTITY_TYPE_ID`, and `ENTITY_ID`: they identify the address. Also pass all text address fields whose values you want to retain or change. If you pass only some of the text fields, the rest are cleared. If you pass none of the `ADDRESS_1`, `ADDRESS_2`, `CITY`, `POSTAL_CODE`, `REGION`, `PROVINCE`, and `COUNTRY` fields, the values may be filled from the specified or previously linked `location` address. The `COUNTRY_CODE` field alone does not prevent this ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TYPE_ID***
[`integer`](../../../data-types.md) | Identifier for the address type. Enumeration element "Address Type".

Enumeration elements for "Address Type" can be retrieved using the method [crm.enum.addresstype](../../auxiliary/enum/crm-enum-address-type.md)
||
|| **ENTITY_TYPE_ID***
[`integer`](../../../data-types.md) | Identifier for the parent object type.

Object type identifiers can be retrieved using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md).

Specify a requisite (`8`) or a lead (`1`). For a requisite address of a company or contact, pass type `8` and the ID of the requisite itself: the method looks up the address by the specified `ENTITY_TYPE_ID` and `ENTITY_ID`
||
|| **ENTITY_ID***
[`integer`](../../../data-types.md) | Identifier of the address owner: a requisite or a lead ||
|| **ADDRESS_1**
[`string`](../../../data-types.md) | Street, house, building, structure ||
|| **ADDRESS_2**
[`string`](../../../data-types.md) | Apartment / office ||
|| **CITY**
[`string`](../../../data-types.md) | City ||
|| **POSTAL_CODE**
[`string`](../../../data-types.md) | Postal code ||
|| **REGION**
[`string`](../../../data-types.md) | District ||
|| **PROVINCE**
[`string`](../../../data-types.md) | State ||
|| **COUNTRY**
[`string`](../../../data-types.md) | Country ||
|| **COUNTRY_CODE**
[`string`](../../../data-types.md) | Country code from the location directory of the `sale` module. If the code is not found, does not match the country name in `COUNTRY`, or the resulting `COUNTRY` value is empty, the country code is cleared
||
|| **LOC_ADDR_ID**
[`integer`](../../../data-types.md) |
Identifier of the address in the `location` module linked to the CRM address. Lets you use location data that the CRM text fields do not have.

The condition for filling fields from `location` is described in the [fields](#fields) parameter. When address fields are passed, only the corresponding passed fields are updated in `location`. If the specified `location` object is linked to another address, a copy of it is created.

CRM fields cleared during a partial update may come back from `location` on the next call without the address fields listed in `fields`; to clear them in `location` as well, pass empty strings
||
|#

## Code Examples

In the examples, the street and city of a requisite address are updated.

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TYPE_ID":1,"ENTITY_TYPE_ID":8,"ENTITY_ID":1,"ADDRESS_1":"Street, 261","CITY":"Los Angeles"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.address.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TYPE_ID":1,"ENTITY_TYPE_ID":8,"ENTITY_ID":1,"ADDRESS_1":"Street, 261","CITY":"Los Angeles"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.address.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'crm.address.update',
        params: {
          fields: {
            TYPE_ID: 1,
            ENTITY_TYPE_ID: 8,
            ENTITY_ID: 1,
            ADDRESS_1: 'Street, 261',
            CITY: 'Los Angeles',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Address updated:', result)
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
      async function updateAddress() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.address.update',
            params: {
              fields: {
                TYPE_ID: 1,
                ENTITY_TYPE_ID: 8,
                ENTITY_ID: 1,
                ADDRESS_1: 'Street, 261',
                CITY: 'Los Angeles',
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
          console.info('Address updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateAddress)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.address.update(
            fields={
                "TYPE_ID": 1,
                "ENTITY_TYPE_ID": 8,
                "ENTITY_ID": 1,
                "ADDRESS_1": "Street, 261",
                "CITY": "Los Angeles",
            },
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
                'crm.address.update',
                [
                    'fields' => [
                        'TYPE_ID'        => 1,
                        'ENTITY_TYPE_ID' => 8,
                        'ENTITY_ID'      => 1,
                        'ADDRESS_1'      => 'Street, 261',
                        'CITY'           => 'Los Angeles',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating address: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.address.update",
        {
            fields:
            {
                "TYPE_ID": 1,
                "ENTITY_TYPE_ID": 8,
                "ENTITY_ID": 1,
                "ADDRESS_1": "Street, 261",
                "CITY": "Los Angeles"
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.address.update',
        [
            'fields' => [
                'TYPE_ID' => 1,
                'ENTITY_TYPE_ID' => 8,
                'ENTITY_ID' => 1,
                'ADDRESS_1' => 'Street, 261',
                'CITY' => 'Los Angeles'
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
    res, err := client.Core().Call(ctx, "crm.address.update", b24.Params{
    	"fields": b24.Params{
    		"TYPE_ID":        1,
    		"ENTITY_TYPE_ID": 8,
    		"ENTITY_ID":      1,
    		"ADDRESS_1":      "Street, 261",
    		"CITY":           "Los Angeles",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.address.update: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1712922620.724857,
        "finish": 1712922623.393783,
        "duration": 2.6689260005950928,
        "processing": 2.210068941116333,
        "date_start": "2024-04-12T14:50:20+02:00",
        "date_finish": "2024-04-12T14:50:23+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | `true` — the address update request completed
||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "ENTITY_ID is not defined or invalid."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | `TYPE_ID is not defined or invalid` | The address type identifier is not specified or has an invalid value ||
|| `400` | Empty value | `ENTITY_TYPE_ID is not defined or invalid` | The address owner type identifier is not specified or has an invalid value ||
|| `400` | Empty value | `ENTITY_ID is not defined or invalid` | The address owner identifier is not specified or has an invalid value ||
|| `400` | Empty value | `TypeAddress not found` | No address with the specified `TYPE_ID`, `ENTITY_TYPE_ID`, and `ENTITY_ID` was found ||
|| `400` | Empty value | `Access denied` | No permission to edit the address owner ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-address-add.md)
- [{#T}](./crm-address-list.md)
- [{#T}](./crm-address-delete.md)
- [{#T}](./crm-address-fields.md)
- [{#T}](./index.md)
