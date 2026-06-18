# Get Address Types crm.enum.addresstype

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.enum.addresstype` returns a list of address types. Use the `ID` of the address type as the value for the `TYPE_ID` parameter in the methods [crm.address.*](../../requisites/addresses/index.md).

## Method Parameters

No parameters.

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.enum.addresstype
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"auth":"**put_access_token_here**"}' \
         https://**put_your_bitrix24_address**/rest/crm.enum.addresstype
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each AddressTypeItem returned in result[]
    type AddressTypeItem = {
      ID: number
      NAME: string
      SYMBOL_CODE: string | null
      SYMBOL_CODE_SHORT: string | null
    }

    try {
      const response = await $b24.actions.v2.call.make<AddressTypeItem[]>({
        method: 'crm.enum.addresstype',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Address types:', result.length, result)
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
      async function fetchAddressTypes() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.enum.addresstype',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Address types:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchAddressTypes)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.enum.addresstype',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling crm.enum.addresstype: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.enum.addresstype",
        {},
        function(result) {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.enum.addresstype',
        []
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
"result": [
    {
     "ID": 11,
     "NAME": "Delivery Address",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    },
    {
     "ID": 1,
     "NAME": "Actual Address",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    },
    {
     "ID": 6,
     "NAME": "Legal Address",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    },
    {
     "ID": 4,
     "NAME": "Registration Address",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    },
    {
     "ID": 8,
     "NAME": "Correspondence Address",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    },
    {
     "ID": 9,
     "NAME": "Beneficiary Address",
     "SYMBOL_CODE": null,
     "SYMBOL_CODE_SHORT": null
    }
],
"time": {
    "start": 1750152255.931318,
    "finish": 1750152255.967967,
    "duration": 0.03664898872375488,
    "processing": 0.0003609657287597656,
    "date_start": "2025-06-17T12:24:15+02:00",
    "date_finish": "2025-06-17T12:24:15+02:00",
    "operating_reset_at": 1750152855,
    "operating": 0
}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md) | Array of address types [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Fields of the result array {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the address type ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the address type ||
|| **SYMBOL_CODE**
[`string`](../../../data-types.md) | Symbolic code ||
|| **SYMBOL_CODE_SHORT**
[`string`](../../../data-types.md) | Short symbolic code ||
|#

## Error Handling

The method does not return errors.

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)