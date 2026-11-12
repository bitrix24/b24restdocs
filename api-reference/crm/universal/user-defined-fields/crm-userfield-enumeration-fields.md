# Get Field Descriptions for Custom Field Type enumeration (List) crm.userfield.enumeration.fields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.userfield.enumeration.fields` returns descriptions of the list element fields for a custom field of the `enumeration` type.

Elements in this format are passed in the `LIST` key of the `crm.*.userfield.add` and `crm.*.userfield.update` methods, for example [crm.deal.userfield.add](../../deals/user-defined-fields/crm-deal-userfield-add.md), and are returned in the `crm.*.userfield.get` and `crm.*.userfield.list` responses. The REST 3.0 method `crm.<type>.field.custom.add` does not create fields of the `enumeration` type; for them, use the deprecated `crm.*.userfield.add` methods or [userfieldconfig.add](../userfieldconfig/userfieldconfig-add.md).

## Method Parameters

No parameters.

## Code Examples

{% include [Example Notes](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.userfield.enumeration.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.userfield.enumeration.fields
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldInfo = {
      type: string
      title: string
      isReadOnly?: boolean
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type EnumerationFieldsResult = Record<string, FieldInfo>

    try {
      const response = await $b24.actions.v2.call.make<EnumerationFieldsResult>({
        method: 'crm.userfield.enumeration.fields',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Enumeration field names:', Object.keys(result))
        console.info('ID field:', result['ID'])
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
      async function getEnumerationFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.userfield.enumeration.fields',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Enumeration field names:', Object.keys(result))
          console.info('ID field:', result['ID'])
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getEnumerationFields)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.userfield.enumeration.fields().response
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
                'crm.userfield.enumeration.fields',
                []
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
        "crm.userfield.enumeration.fields",
        {},
        function(result)
        {
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
        'crm.userfield.enumeration.fields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.userfield.enumeration.fields", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.userfield.enumeration.fields: %w", err)
    }

    keys, ok := b24.Keys(res.Result)
    if !ok {
    	return fmt.Errorf("expected an object in the response")
    }
    fmt.Println("fields in response:", len(keys))
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "ID": {
            "type": "int",
            "title": "ID",
            "isReadOnly": true
        },
        "SORT": {
            "type": "int",
            "title": "Sorting"
        },
        "VALUE": {
            "type": "string",
            "title": "Value"
        },
        "DEF": {
            "type": "string",
            "title": "Default Value"
        },
        "DEL": {
            "type": "string",
            "title": "Element Deletion Flag"
        }
    },
    "time": {
        "start": 1773992153,
        "finish": 1773992153.048253,
        "duration": 0.04825305938720703,
        "processing": 0,
        "date_start": "2026-03-20T10:35:53+02:00",
        "date_finish": "2026-03-20T10:35:53+02:00",
        "operating_reset_at": 1773992753,
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

#### The result Object {#result}

A key of the `result` object is a list element field, and the value is the field description [(detailed description)](#descriptor). The table shows the value type that the field accepts in `LIST`. The complete element description is the [uf_enum_element](../../../data-types.md#uf_enum_element) object.

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Element identifier. Set by the system: not passed when creating a field. In `update`, pass `ID` to change or delete an existing element. An element without `ID` is added to the list as a new one ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting order. If not passed or less than 1, the element gets `500` ||
|| **VALUE**
[`string`](../../../data-types.md) | Element text. Elements with an empty `VALUE` are skipped when the field is created ||
|| **DEF**
[`char`](../../../data-types.md) | Default value flag: `Y` or `N`. If the field is not multiple, only the first element with `DEF: Y` becomes the default value ||
|| **DEL**
[`char`](../../../data-types.md) | Deletion flag: `Y` together with `ID` in `update` deletes the element. Elements not passed in `update` are retained ||
|#

In addition to these fields, a list element accepts `XML_ID`, the external element identifier. The method does not describe it. In the response, the element `XML_ID` is returned by `crm.*.userfield.list` but not by `crm.*.userfield.get`.

#### Field Description {#descriptor}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../../data-types.md) | Value type: `int` or `string` ||
|| **title**
[`string`](../../../data-types.md) | Field name in the Bitrix24 interface language ||
|| **isReadOnly**
[`boolean`](../../../data-types.md) | The value is set by the system. Only `ID` has this key ||
|#

## Error Handling

The method has no errors of its own: it does not check CRM permissions and takes no parameters.

{% include notitle [error handling](../../../../_includes/error-info.md) %}

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-userfield-types.md)
- [{#T}](./crm-userfield-fields.md)
- [{#T}](./crm-userfield-settings-fields.md)
- [{#T}](../../deals/user-defined-fields/crm-deal-userfield-add.md)
- [{#T}](./index.md)