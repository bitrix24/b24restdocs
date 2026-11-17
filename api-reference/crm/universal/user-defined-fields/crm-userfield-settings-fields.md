# Get Field Settings Description for Custom Field Type crm.userfield.settings.fields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.userfield.settings.fields` returns the settings description for the specified custom field type.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type***
[`string`](../../../data-types.md) | Code of the custom field type from the response of the [crm.userfield.types](./crm-userfield-types.md) method, for example `string` or `enumeration`. The value is case-sensitive: for `String`, the method returns an empty array.

Settings by type [(detailed description)](#settings) ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"double"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.userfield.settings.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"double","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.userfield.settings.fields
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type UserfieldSettingsItem = {
      type: string
      title: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type UserfieldSettingsResult = Record<string, UserfieldSettingsItem>

    try {
      const response = await $b24.actions.v2.call.make<UserfieldSettingsResult>({
        method: 'crm.userfield.settings.fields',
        params: {
          type: 'double',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(Object.keys(result), result)
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
      async function fetchUserfieldSettingsFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.userfield.settings.fields',
            params: {
              type: 'double',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(Object.keys(result), result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchUserfieldSettingsFields)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.userfield.settings.fields(
            type="double",
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
                'crm.userfield.settings.fields',
                [
                    'type' => 'double'
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
        "crm.userfield.settings.fields",
        {
            type: "double"
        },
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
        'crm.userfield.settings.fields',
        [
            'type' => 'double'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.userfield.settings.fields", b24.Params{
    	"type": "double",
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.userfield.settings.fields: %w", err)
    }

    // For types without settings, such as date, result is an empty array []
    if b24.IsEmpty(res.Result) {
    	fmt.Println("the type has no settings")
    	return nil
    }
    keys, ok := b24.Keys(res.Result)
    if !ok {
    	return fmt.Errorf("expected an object in the response")
    }
    fmt.Println("settings in response:", len(keys))
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "DEFAULT_VALUE": {
            "type": "double",
            "title": "Default Value"
        },
        "PRECISION": {
            "type": "int",
            "title": "Precision"
        }
    },
    "time": {
        "start": 1752132864.335154,
        "finish": 1752132864.366912,
        "duration": 0.03175783157348633,
        "processing": 0.002053976058959961,
        "date_start": "2025-07-10T10:34:24+02:00",
        "date_finish": "2025-07-10T10:34:24+02:00",
        "operating_reset_at": 1752133464,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Description of the settings for the `type` type: the key is the setting code, and the value is the setting description [(detailed description)](#descriptor).

If the type has no settings that the method describes, or the type is unknown, an empty array `[]` is returned [(detailed description)](#settings) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Settings by Field Type {#settings}

The set of `result` keys depends on the field type. The keys are passed in `SETTINGS` when creating or updating a field, for example in [crm.deal.userfield.add](../../deals/user-defined-fields/crm-deal-userfield-add.md). The REST 3.0 method [crm.&lt;type&gt;.field.custom.add](../../rest-v3/fields/crm-entity-field-custom-add-rest-v3.md) accepts the same settings in camelCase, for example `iblockId` instead of `IBLOCK_ID`. It does not create fields of the `enumeration` and `file` types or of application types.

#|
|| **Field type** | **result keys** ||
|| `string` | `DEFAULT_VALUE` (`string`) — default value, `ROWS` (`int`) — number of rows in the input box ||
|| `integer` | `DEFAULT_VALUE` (`int`) — default value ||
|| `double` | `DEFAULT_VALUE` (`double`) — default value, `PRECISION` (`int`) — precision ||
|| `boolean` | `DEFAULT_VALUE` (`int`) — default value, `DISPLAY` (`string`) — appearance ||
|| `datetime` | `DEFAULT_VALUE` (`datetime`) — default value ||
|| `enumeration` | `DISPLAY` (`string`) — appearance, `LIST_HEIGHT` (`int`) — list height. List elements are described by the [crm.userfield.enumeration.fields](./crm-userfield-enumeration-fields.md) method ||
|| `iblock_section`, `iblock_element` | `DEFAULT_VALUE` (`int`) — default value, `IBLOCK_ID` (`int`) — information block ID, `IBLOCK_TYPE_ID` (`string`) — information block type ID, `DISPLAY` (`string`) — appearance, `LIST_HEIGHT` (`int`) — list height, `ACTIVE_FILTER` (`char`) — show active elements only ||
|| `crm_status` | `ENTITY_TYPE` (`string`) — the CRM directory to which the field is bound ||
|| `crm` | `LEAD`, `CONTACT`, `COMPANY`, `DEAL` (`char`) — the CRM objects to which the value can be bound ||
|| `date`, `money`, `url`, `address`, `file`, `employee`, application types | Empty array `[]` ||
|#

#### Setting Description {#descriptor}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../../data-types.md) | Setting value type: `string`, `int`, `double`, `datetime`, or `char` ||
|| **title**
[`string`](../../../data-types.md) | Setting name in the Bitrix24 interface language ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Parameter 'type' is not specified or empty."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty Value | Parameter 'type' is not specified or empty | The parameter `type` is not provided or an empty value is passed ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-userfield-types.md)
- [{#T}](./crm-userfield-fields.md)
- [{#T}](./crm-userfield-enumeration-fields.md)
- [{#T}](../../deals/user-defined-fields/crm-deal-userfield-add.md)
- [{#T}](../../rest-v3/fields/crm-entity-field-custom-add-rest-v3.md)
- [{#T}](./index.md)