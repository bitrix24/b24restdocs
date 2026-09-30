# Get Description of Multiple Fields crm.multifield.fields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with read access to leads, deals, or other CRM objects, including those in digital workspaces

The method `crm.multifield.fields` describes the fields of the [crm_multifield](../../data-types.md#crm_multifield) object. This object stores a single phone number, e-mail, website, or messenger value and consists of the `ID`, `TYPE_ID`, `VALUE`, and `VALUE_TYPE` fields. For each field, the method returns the data type, the name, and the read-only flag. For example, the response shows that you pass `VALUE` and `VALUE_TYPE`, while Bitrix24 fills in `ID` automatically. The method does not return the allowed `VALUE_TYPE` values — they are listed in the [VALUE_TYPE Values](#value-type) table.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.multifield.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"auth":"**put_access_token_here**"}' \
         https://**put_your_bitrix24_address**/rest/crm.multifield.fields
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldDescription = {
      type: string
      isRequired: boolean
      isReadOnly: boolean
      isImmutable: boolean
      isMultiple: boolean
      isDynamic: boolean
      title: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type MultifieldFieldsResult = Record<string, FieldDescription>

    try {
      const response = await $b24.actions.v2.call.make<MultifieldFieldsResult>({
        method: 'crm.multifield.fields',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(Object.keys(result), result['ID'])
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
      async function getMultifieldFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.multifield.fields',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(Object.keys(result), result['ID'])
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getMultifieldFields)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.multifield.fields().response
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
                'crm.multifield.fields',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        foreach ($result as $code => $field) {
            echo $code . ' — ' . $field['title'] . ' (' . $field['type'] . ')' . PHP_EOL;
        }
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching multifield fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.multifield.fields",
        {},
        function(result) {
            if(result.error())
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
        'crm.multifield.fields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.multifield.fields", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.multifield.fields: %w", err)
    }

    keys, ok := b24.Keys(res.Result)
    if !ok {
    	return fmt.Errorf("expected an object in the response")
    }
    fmt.Println("fields in response:", len(keys))
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "ID": {
            "type": "int",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "ID"
        },
        "TYPE_ID": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Field Type"
        },
        "VALUE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Value"
        },
        "VALUE_TYPE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Value Type"
        }
    },
    "time": {
        "start": 1750684622.069806,
        "finish": 1750684622.120529,
        "duration": 0.05072283744812012,
        "processing": 0.00037217140197753906,
        "date_start": "2025-06-23T16:17:02+02:00",
        "date_finish": "2025-06-23T16:17:02+02:00",
        "operating_reset_at": 1750685222,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with field descriptions [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Fields of the result object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`object`](../../../data-types.md) | Identifier of the multiple field value ||
|| **TYPE_ID**
[`object`](../../../data-types.md) | Type of the multiple field: `PHONE`, `EMAIL`, `WEB`, `IM`, `LINK` ||
|| **VALUE**
[`object`](../../../data-types.md) | Value of the multiple field ||
|| **VALUE_TYPE**
[`object`](../../../data-types.md) | Type of the multiple field value, for example `MOBILE` or `WORK`. Allowed values depend on `TYPE_ID` and are listed in the [VALUE_TYPE Values](#value-type) table ||
|#

#### Description of Field Characteristics

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../../data-types.md) | Data type of the field ||
|| **isRequired**
[`boolean`](../../../data-types.md) | Required ||
|| **isReadOnly**
[`boolean`](../../../data-types.md) | Read-only ||
|| **isImmutable**
[`boolean`](../../../data-types.md) | Immutable ||
|| **isMultiple**
[`boolean`](../../../data-types.md) | Multiple ||
|| **isDynamic**
[`boolean`](../../../data-types.md) | Dynamic ||
|| **title**
[`string`](../../../data-types.md) | Name of the field ||
|#

#### VALUE_TYPE Values {#value-type}

#|
|| **TYPE_ID** | **VALUE_TYPE Values** ||
|| `PHONE` — phone | `WORK` — work, `MOBILE` — mobile, `FAX` — fax, `HOME` — home, `PAGER` — pager, `MAILING` — SMS marketing, `OTHER` — other ||
|| `EMAIL` — e-mail | `WORK` — work, `HOME` — home, `MAILING` — for newsletters, `OTHER` — other ||
|| `WEB` — website | `WORK` — corporate, `HOME` — personal, `FACEBOOK`, `VK`, `LIVEJOURNAL`, `TWITTER`, `OTHER` — other ||
|| `IM` — messenger | `FACEBOOK`, `TELEGRAM`, `VK`, `VIBER`, `INSTAGRAM`, `BITRIX24` — Bitrix24 Network, `OPENLINE` — Live Chat, `IMOL` — Open Channel, `OTHER` — other ||
|| `LINK` — link | `USER` — user ||
|#

The `SKYPE`, `ICQ`, `MSN`, and `JABBER` messenger values are deprecated. Bitrix24 offers them only if these messengers were previously used in your CRM.

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
- [{#T}](../index.md)
