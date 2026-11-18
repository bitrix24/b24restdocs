# Add Address crm.address.add

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

Adds a new address for a requisite or lead. For the user, this address appears as the address of a contact, company, or lead.

Multiple addresses of different [types](../../auxiliary/enum/crm-enum-address-type.md) can be created, linked to a single requisite.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | An object with fields for adding the address [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TYPE_ID***
[`integer`](../../../data-types.md) | Identifier of the address type. Enumeration element "Address Type".

Elements of the enumeration "Address Type" can be obtained using the method [crm.enum.addresstype](../../auxiliary/enum/crm-enum-address-type.md)
||
|| **ENTITY_TYPE_ID***
[`integer`](../../../data-types.md) | Identifier of the parent object's type.

Identifiers of object types can be obtained using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md).

An address can be linked to a requisite (`8`) or a lead (`1`). For backward compatibility, you can pass a company (`4`) or a contact (`3`): the address is retained in the requisite where this company or contact already has a primary address of the same type. If there is none, it goes to the requisite selected in the card, otherwise to the first requisite in ascending order of `SORT` and `ID`. If there are no requisites, Bitrix24 creates a new one
||
|| **ENTITY_ID***
[`integer`](../../../data-types.md) | Identifier of the parent object: a [requisite](../universal/index.md) or a [lead](../../leads/index.md). With `ENTITY_TYPE_ID=4` or `3`, pass the company or contact ID, respectively ||
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
[`string`](../../../data-types.md) | Country code. Retained only if `COUNTRY` exactly matches the country name for this code. If `COUNTRY` is not passed or does not match, the code is cleared
||
|| **LOC_ADDR_ID**
[`integer`](../../../data-types.md) | Identifier of the location address.

Lets you use an existing address of the `location` module, including data that the CRM address fields do not have. If the location address is already linked to another object, a copy of it is created.

If no string address fields are passed, they are filled from the location address; to create a CRM address, at least one of them must be non-empty. If at least one of the `ADDRESS_1`, `ADDRESS_2`, `CITY`, `POSTAL_CODE`, `REGION`, `PROVINCE`, `COUNTRY` fields is passed, only the passed fields are retained in CRM. They also update the corresponding fields of the location address
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TYPE_ID":1,"ENTITY_TYPE_ID":8,"ENTITY_ID":1,"ADDRESS_1":"4 Peace Avenue","ADDRESS_2":"Drama Theater","CITY":"City","POSTAL_CODE":"000000","REGION":"Urban District","PROVINCE":"Region","COUNTRY":"USA"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.address.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TYPE_ID":1,"ENTITY_TYPE_ID":8,"ENTITY_ID":1,"ADDRESS_1":"4 Peace Avenue","ADDRESS_2":"Drama Theater","CITY":"City","POSTAL_CODE":"000000","REGION":"Urban District","PROVINCE":"Region","COUNTRY":"USA"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.address.add
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
        method: 'crm.address.add',
        params: {
          fields: {
            TYPE_ID: 1,            // Address type, see crm.enum.addresstype
            ENTITY_TYPE_ID: 8,     // Parent object type (requisite or lead)
            ENTITY_ID: 1,          // Requisite identifier
            ADDRESS_1: '4 Peace Avenue',
            ADDRESS_2: 'Drama Theater',
            CITY: 'City',
            REGION: 'City urban district',
            PROVINCE: 'Region',
            COUNTRY: 'USA',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Address added:', result)
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
      async function addAddress() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.address.add',
            params: {
              fields: {
                TYPE_ID: 1,            // Address type, see crm.enum.addresstype
                ENTITY_TYPE_ID: 8,     // Parent object type (requisite or lead)
                ENTITY_ID: 1,          // Requisite identifier
                ADDRESS_1: '4 Peace Avenue',
                ADDRESS_2: 'Drama Theater',
                CITY: 'City',
                REGION: 'City urban district',
                PROVINCE: 'Region',
                COUNTRY: 'USA',
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
          console.info('Address added:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addAddress)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.address.add(
            fields={
                "TYPE_ID": 1,
                "ENTITY_TYPE_ID": 8,
                "ENTITY_ID": 1,
                "ADDRESS_1": "15 Main Street",
                "ADDRESS_2": "Suite 300",
                "CITY": "Boston",
                "POSTAL_CODE": "02110",
                "REGION": "Suffolk County",
                "PROVINCE": "Massachusetts",
                "COUNTRY": "United States",
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
                'crm.address.add',
                [
                    'fields' => [
                        'TYPE_ID'        => 1,
                        'ENTITY_TYPE_ID' => 8,
                        'ENTITY_ID'      => 1,
                        'ADDRESS_1' => '4 Peace Avenue',
                        'ADDRESS_2' => 'Drama Theater',
                        'CITY' => 'City',
                        'POSTAL_CODE' => '00000',
                        'REGION' => 'Urban District',
                        'PROVINCE' => 'Region',
                        'COUNTRY' => 'USA',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding address: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.address.add",
        {
            fields:
            {
                "TYPE_ID": 1,            // Address type, see crm.enum.addresstype
                "ENTITY_TYPE_ID": 8,     // Object type (requisite or lead)
                "ENTITY_ID": 1,          // Identifier of the requisite
                "ADDRESS_1": "4 Peace Avenue",
                "ADDRESS_2": "Drama Theater",
                "CITY": "City",
                "POSTAL_CODE": "00000",
                "REGION": "Urban District",
                "PROVINCE": "Region",
                "COUNTRY": "USA",
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
        'crm.address.add',
        [
            'fields' => [
                'TYPE_ID' => 1,
                'ENTITY_TYPE_ID' => 8,
                'ENTITY_ID' => 1,
                'ADDRESS_1' => '4 Peace Avenue',
                'ADDRESS_2' => 'Drama Theater',
                'CITY' => 'City',
                'POSTAL_CODE' => '00000',
                'REGION' => 'Urban District',
                'PROVINCE' => 'Region',
                'COUNTRY' => 'USA',
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
    res, err := client.Core().Call(ctx, "crm.address.add", b24.Params{
    	"fields": b24.Params{
    		"TYPE_ID":        1,
    		"ENTITY_TYPE_ID": 8,
    		"ENTITY_ID":      1,
    		"ADDRESS_1":      "4 Peace Avenue",
    		"ADDRESS_2":      "Drama Theater",
    		"CITY":           "City",
    		"POSTAL_CODE":    "236036",
    		"REGION":         "Urban District",
    		"PROVINCE":       "Region",
    		"COUNTRY":        "USA",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.address.add: %w", err)
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
[`boolean`](../../../data-types.md) | The value `true` means that the request completed without an error.

If you pass only `TYPE_ID`, `ENTITY_TYPE_ID`, and `ENTITY_ID`, the method returns `true`, but no address is created. Pass a non-empty text address field or the `LOC_ADDR_ID` of a location address with non-empty address fields, then check the result with the [crm.address.list](./crm-address-list.md) method
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
|| `400` | Empty value | `TYPE_ID is not defined or invalid` | The address type is not specified or is invalid ||
|| `400` | Empty value | `ENTITY_TYPE_ID is not defined or invalid` | The parent object type is not specified or is invalid ||
|| `400` | Empty value | `ENTITY_ID is not defined or invalid` | The parent object identifier is not specified or is not a positive number ||
|| `400` | Empty value | `TypeAddress exists` | The parent object already has an address of this type ||
|| `400` | Empty value | `Access denied` | No permission to edit the address owner, or the parent object type does not support adding an address ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-address-update.md)
- [{#T}](./crm-address-list.md)
- [{#T}](./crm-address-delete.md)
- [{#T}](./crm-address-fields.md)
- [{#T}](./index.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-company-with-requisite.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-contact-with-requisite.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-deal-with-choice-of-requisite.md)
