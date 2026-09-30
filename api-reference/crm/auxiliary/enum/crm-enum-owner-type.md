# Get CRM Object Types crm.enum.ownertype

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with read access to leads, deals, or other CRM objects, including those in digital workspaces

The method `crm.enum.ownertype` returns the numeric IDs of CRM object types and SPAs. Pass the ID in the `entityTypeId` parameter of the universal methods [crm.item.*](../../universal/index.md) and in the `OWNER_TYPE_ID` or `ownerTypeId` parameter of the [activity](../../timeline/activities/index.md) methods. For example, to retrieve a deal with the [crm.item.get](../../universal/crm-item-get.md) method, pass `entityTypeId: 2`, and for an SPA, pass its ID, for example, `177`.

Universal methods do not work with all types in the list: they do not support the old invoice with ID `5` and requisites with ID `8`, and return the `ENTITY_TYPE_NOT_SUPPORTED` error.

{% note info " " %}

The identifiers of SPA in each Bitrix24 are unique and may differ from those provided in the example.

{% endnote %}

## Method Parameters

No parameters.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.enum.ownertype
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"auth":"**put_access_token_here**"}' \
         https://**put_your_bitrix24_address**/rest/crm.enum.ownertype
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each item returned in result[]
    type OwnerTypeItem = {
      ID: number
      NAME: string
      SYMBOL_CODE: string
      SYMBOL_CODE_SHORT: string
    }

    try {
      const response = await $b24.actions.v2.call.make<OwnerTypeItem[]>({
        method: 'crm.enum.ownertype',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Owner types:', result.length, result)
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
      async function getOwnerTypes() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.enum.ownertype',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Owner types:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getOwnerTypes)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.enum.ownertype().response
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
                'crm.enum.ownertype',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        foreach ($result as $ownerType) {
            echo $ownerType['ID'] . ' — ' . $ownerType['NAME'] . ' (' . $ownerType['SYMBOL_CODE'] . ')' . PHP_EOL;
        }
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling crm.enum.ownertype: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.enum.ownertype",
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
        'crm.enum.ownertype',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.enum.ownertype", nil)
    if err != nil {
    	return fmt.Errorf("crm.enum.ownertype: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
"result": [
    {
     "ID": 1,
     "NAME": "Lead",
     "SYMBOL_CODE": "LEAD",
     "SYMBOL_CODE_SHORT": "L"
    },
    {
     "ID": 2,
     "NAME": "Deal",
     "SYMBOL_CODE": "DEAL",
     "SYMBOL_CODE_SHORT": "D"
    },
    {
     "ID": 3,
     "NAME": "Contact",
     "SYMBOL_CODE": "CONTACT",
     "SYMBOL_CODE_SHORT": "C"
    },
    {
     "ID": 4,
     "NAME": "Company",
     "SYMBOL_CODE": "COMPANY",
     "SYMBOL_CODE_SHORT": "CO"
    },
    {
     "ID": 5,
     "NAME": "Invoice (old version)",
     "SYMBOL_CODE": "INVOICE",
     "SYMBOL_CODE_SHORT": "I"
    },
    {
     "ID": 31,
     "NAME": "Invoice",
     "SYMBOL_CODE": "SMART_INVOICE",
     "SYMBOL_CODE_SHORT": "SI"
    },
    {
     "ID": 7,
     "NAME": "Quote",
     "SYMBOL_CODE": "QUOTE",
     "SYMBOL_CODE_SHORT": "Q"
    },
    {
     "ID": 8,
     "NAME": "Billing Details",
     "SYMBOL_CODE": "REQUISITE",
     "SYMBOL_CODE_SHORT": "RQ"
    },
    {
     "ID": 36,
     "NAME": "Document",
     "SYMBOL_CODE": "SMART_DOCUMENT",
     "SYMBOL_CODE_SHORT": "DO"
    },
    {
     "ID": 39,
     "NAME": "Company Document",
     "SYMBOL_CODE": "SMART_B2E_DOC",
     "SYMBOL_CODE_SHORT": "SBD"
    },
    {
     "ID": 177,
     "NAME": "Equipment Purchase",
     "SYMBOL_CODE": "DYNAMIC_177",
     "SYMBOL_CODE_SHORT": "Tb1"
    },
    {
     "ID": 156,
     "NAME": "Purchase",
     "SYMBOL_CODE": "DYNAMIC_156",
     "SYMBOL_CODE_SHORT": "T9c"
    }
],
"time": {
    "start": 1750153184.228934,
    "finish": 1750153184.262921,
    "duration": 0.03398704528808594,
    "processing": 0.0008471012115478516,
    "date_start": "2025-06-17T12:39:44+03:00",
    "date_finish": "2025-06-17T12:39:44+03:00",
    "operating_reset_at": 1750153784,
    "operating": 0
}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md) | Array with object types [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Fields of the Result Array {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the object type, for example, `2` — deal. For an SPA, its `entityTypeId` ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the object type. For an SPA, the name it was given in Bitrix24 ||
|| **SYMBOL_CODE**
[`string`](../../../data-types.md) | Symbolic code of the object type, for example, `DEAL`. For an SPA, `DYNAMIC_` followed by its `ID`, for example, `DYNAMIC_177` ||
|| **SYMBOL_CODE_SHORT**
[`string`](../../../data-types.md) | Short symbolic code, for example, `D` for a deal. For an SPA, the letter `T` followed by the `ID` in hexadecimal: `Tb1` for `177`. The short code is also required when linking a task to a CRM object ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | Access denied. | The user does not have read access to CRM objects, including those in digital workspaces ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](../../../../tutorials/tasks/how-to-connect-task-to-spa.md)
