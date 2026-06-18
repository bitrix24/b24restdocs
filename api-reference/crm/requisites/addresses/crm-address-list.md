# Get a List of Addresses by Filter crm.address.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a list of addresses based on the filter.

Addresses are moved to the details. In the CRM card, they are displayed as a separate field.

Multiple details can be linked to a CRM object. Within a detail, there can be several addresses (each of a different type).

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | An array of fields to select (see [address fields](#fields)).

If the array is not provided or an empty array is passed, all available address fields will be selected. ||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering selected addresses in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to [address fields](#fields).

An additional prefix can be assigned to the key to clarify the filter's behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol must be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol must be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equals, exact match (used by default)
- `!=` — not equal
- `!` — not equal ||
|| **order**
[`object`](../../../data-types.md) | An object for sorting selected addresses in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to [address fields](#fields).

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../../data-types.md) | This parameter is used to manage pagination.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number 
||
|#

### Description of Address Fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the address type. Enumeration element "Address Type".

Enumeration elements for "Address Type" can be obtained using the method [crm.enum.addresstype](../../auxiliary/enum/crm-enum-address-type.md)
||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the parent object's type.

Object type identifiers can be obtained using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md).

Addresses can only be linked to Details (and details to companies or contacts) or Leads.

For backward compatibility, the ability to link Addresses to Contacts or Companies is retained. However, this link is only possible on some older accounts where the old address handling mode was specifically enabled by support.
||
|| **ENTITY_ID**
[`string`](../../../data-types.md) | Identifier of the parent object ||
|| **ADDRESS_1**
[`string`](../../../data-types.md) | Street, house, building, structure ||
|| **ADDRESS_2**
[`string`](../../../data-types.md) | Apartment / office ||
|| **CITY**
[`string`](../../../data-types.md) | City ||
|| **POSTAL_CODE**
[`string`](../../../data-types.md) | Postal code ||
|| **REGION**
[`string`](../../../data-types.md) | Region ||
|| **PROVINCE**
[`string`](../../../data-types.md) | Province ||
|| **COUNTRY**
[`string`](../../../data-types.md) | Country ||
|| **COUNTRY_CODE**
[`string`](../../../data-types.md) | Country code.

Not used, retained for backward compatibility. An empty string can be specified as a value.
||
|| **LOC_ADDR_ID**
[`integer`](../../../data-types.md) | Identifier of the location address.

This field contains the identifier of the address object in the `Location` module, linked to the CRM address object. Each CRM address corresponds to an address object in the `location` module. This can be used to copy an existing address into CRM with location information that is not present in the CRM address fields.

If the identifier of the `location` module address is specified when creating an address, a copy of the `location` address is created and linked to the created CRM address. If no values are specified for the string fields of the address in this case, they will be filled from the location address.

If at least one string field was specified, only the specified fields will be saved in the CRM address, and their values will overwrite the corresponding values in the location address object. The same behavior will occur when updating the address.
||
|| **ANCHOR_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the main parent object's type.

This field is for internal use. The value is automatically filled when adding an address.

Object type identifiers can be obtained using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md).

This field contains the identifier of the parent object's type of the detail (company or contact) if the address is linked to the detail. If the address is linked to a lead, this value will be the lead type identifier.
||
|| **ANCHOR_ID**
[`integer`](../../../data-types.md) | This field is for internal use. The value is automatically filled when adding an address.

This field contains the identifier of the parent object of the detail (company or contact) if the address is linked to the detail. If the address is linked to a lead, this value will be the lead identifier.
||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

Searching for addresses linked to the Detail type:

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"TYPE_ID":"asc"},"filter":{"ENTITY_TYPE_ID":8,"ENTITY_ID":7335},"limit":10}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.address.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"TYPE_ID":"asc"},"filter":{"ENTITY_TYPE_ID":8,"ENTITY_ID":7335},"limit":10,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.address.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each AddressItem returned in result[]
    type AddressItem = {
      TYPE_ID: string
      ENTITY_TYPE_ID: string
      ENTITY_ID: string
      ADDRESS_1: string
      ADDRESS_2: string
      CITY: string
      POSTAL_CODE: string
      REGION: string
      PROVINCE: string
      COUNTRY: string
      COUNTRY_CODE: string | null
      LOC_ADDR_ID: string
      ANCHOR_TYPE_ID: string
      ANCHOR_ID: string
    }

    try {
      // crm.address.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<AddressItem[]>({
        method: 'crm.address.list',
        params: {
          order: { TYPE_ID: 'asc' },
          filter: { ENTITY_TYPE_ID: 8, ENTITY_ID: 7335 },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Addresses found:', result.length, result)
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
      async function fetchAddressList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.address.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.address.list',
            params: {
              order: { TYPE_ID: 'asc' },
              filter: { ENTITY_TYPE_ID: 8, ENTITY_ID: 7335 },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Addresses found:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchAddressList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.address.list',
                [
                    'order' => ['TYPE_ID' => 'asc'],
                    'filter' => ['ENTITY_TYPE_ID' => 8, 'ENTITY_ID' => 7335],
                    'limit' => 10,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
        if ($result->more()) {
            $result->next();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching address list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.address.list",
        {
            order: { "TYPE_ID": "asc"},
            filter: { "ENTITY_TYPE_ID": 8, "ENTITY_ID": 7335},
            limit: 10
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.dir(result.data());
                if(result.more())
                    result.next();
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.address.list',
        [
            'order' => ['TYPE_ID' => 'asc'],
            'filter' => ['ENTITY_TYPE_ID' => 8, 'ENTITY_ID' => 7335],
            'limit' => 10
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.address.list(
            select=["TYPE_ID", "ENTITY_TYPE_ID", "ENTITY_ID", "ADDRESS_1", "ADDRESS_2", "CITY", "POSTAL_CODE", "COUNTRY"],
            filter={"ENTITY_TYPE_ID": 8, "ENTITY_ID": 7335},
            order={"TYPE_ID": "ASC"},
            start=0,
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

    Example `as_list`

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.address.list(
            select=["TYPE_ID", "ENTITY_TYPE_ID", "ENTITY_ID", "ADDRESS_1", "CITY", "COUNTRY"],
            filter={"ENTITY_TYPE_ID": 8, "ENTITY_ID": 7335},
            order={"TYPE_ID": "ASC"},
        ).as_list().response
        result = bitrix_response.result
        for item in result:
            print(item)
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
{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "TYPE_ID": "1",
            "ENTITY_TYPE_ID": "8",
            "ENTITY_ID": "7335",
            "ADDRESS_1": "Address 1",
            "ADDRESS_2": "701",
            "CITY": "Los Angeles",
            "POSTAL_CODE": "625003",
            "REGION": "California",
            "PROVINCE": "California",
            "COUNTRY": "USA",
            "COUNTRY_CODE": null,
            "LOC_ADDR_ID": "479",
            "ANCHOR_TYPE_ID": "3",
            "ANCHOR_ID": "17192"
        }
    ],
    "total": 1,
    "time": {
        "start": 1716301758.664873,
        "finish": 1716301759.73158,
        "duration": 1.0667071342468262,
        "processing": 0.028820037841796875,
        "date_start": "2024-05-21T16:29:18+02:00",
        "date_finish": "2024-05-21T16:29:19+02:00",
        "operating": 0
    }
}
```

If a contact has 2 different details to which addresses are linked:

HTTP Status: **200**

```json
{
    "result": [
        {
            "TYPE_ID": "1",
            "ENTITY_TYPE_ID": "8",
            "ENTITY_ID": "7335",
            "ADDRESS_1": "Address 2",
            "ADDRESS_2": "701",
            "CITY": "Los Angeles",
            "POSTAL_CODE": "625003",
            "REGION": "California",
            "PROVINCE": "California",
            "COUNTRY": "USA",
            "COUNTRY_CODE": null,
            "LOC_ADDR_ID": "479",
            "ANCHOR_TYPE_ID": "3",
            "ANCHOR_ID": "17192"
        },
        {
            "TYPE_ID": "1",
            "ENTITY_TYPE_ID": "8",
            "ENTITY_ID": "7335",
            "ADDRESS_1": "Address",
            "ADDRESS_2": "2",
            "CITY": "Los Angeles",
            "POSTAL_CODE": "666000",
            "REGION": "California",
            "PROVINCE": "California",
            "COUNTRY": "USA",
            "COUNTRY_CODE": null,
            "LOC_ADDR_ID": "129",
            "ANCHOR_TYPE_ID": "3",
            "ANCHOR_ID": "17192"
        }
    ],
    "total": 2,
    "time": {
        "start": 1716301758.664873,
        "finish": 1716301759.73158,
        "duration": 1.0667071342468262,
        "processing": 0.028820037841796875,
        "date_start": "2024-05-21T16:29:18+02:00",
        "date_finish": "2024-05-21T16:29:19+02:00",
        "operating": 0
    }
}
```

{% note info "Additional Information about the Result" %}

The **ANCHOR_TYPE_ID** field is filled with a value from [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) (in this example, it is Contacts), and the **ANCHOR_ID** field contains the ID of the object (in this case, the Contact).

The **ANCHOR_TYPE_ID** and **ANCHOR_ID** fields in the two examples above are the same, indicating that both addresses belong to the same Contact.

{% endnote %}

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md) | An array of objects with information about the selected addresses. Each element contains the selected [address fields](#fields) ||
|| **total**
[`integer`](../../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **40x**, **50x**

```json
{
    "error":0,
    "error_description":"error"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|  
|| **Code** | **Description** ||
|| `Access denied` | Insufficient access rights to retrieve the list of addresses. No read access to companies, contacts, leads ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-address-add.md)
- [{#T}](./crm-address-update.md)
- [{#T}](./crm-address-delete.md)
- [{#T}](./crm-address-fields.md)

