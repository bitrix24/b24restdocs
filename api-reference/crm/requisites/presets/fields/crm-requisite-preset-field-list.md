# Get a List of All Customizable Fields for a Specified CRM Requisites Template crm.requisite.preset.field.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

Returns a list of all customizable fields for a specific company details template.

## Method Parameters

{% include [Note on parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **preset***
[`object`](../../../../data-types.md) | An object containing the identifier value of the template from which the list of customizable fields is extracted (for example, `{"ID": 27}`). Template identifiers can be obtained using the [crm.requisite.preset.list](../crm-requisite-preset-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"preset":{"ID":27}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.preset.field.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"preset":{"ID":27},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.preset.field.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each item returned in result[]
    type PresetFieldItem = {
      ID: number
      FIELD_NAME: string
      FIELD_TITLE: string
      IN_SHORT_LIST: string
      SORT: number
    }

    try {
      // crm.requisite.preset.field.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<PresetFieldItem[]>({
        method: 'crm.requisite.preset.field.list',
        params: {
          preset: {
            ID: 27,
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Preset fields:', result.length, result)
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
      async function listPresetFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.requisite.preset.field.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.requisite.preset.field.list',
            params: {
              preset: {
                ID: 27,
              },
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
          console.info('Preset fields:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listPresetFields)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.requisite.preset.field.list',
                [
                    'preset' => [
                        'ID' => 27
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching preset field list: ' . $e->getMessage();
    }
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.requisite.preset.field.list(
            preset={
                "ID": 27,
            },
        ).response
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

    Example `as_list`

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.requisite.preset.field.list(
            preset={
                "ID": 27,
            },
        ).as_list().response
        result = bitrix_response.result
        for item in result:
            print(item)
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

    Example `as_list_fast`

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.requisite.preset.field.list(
            preset={
                "ID": 27,
            },
        ).as_list_fast(descending=True).response
        result = bitrix_response.result
        for item in result:
            print(item)
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

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.preset.field.list",
        {
            preset:
            {
                "ID": 27
            }
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
        'crm.requisite.preset.field.list',
        [
            'preset' => ['ID' => 27]
        ]
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
            "ID": 1,
            "FIELD_NAME": "RQ_INN",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "Y",
            "SORT": 510
        },
        {
            "ID": 2,
            "FIELD_NAME": "RQ_COMPANY_NAME",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "Y",
            "SORT": 520
        },
        {
            "ID": 3,
            "FIELD_NAME": "RQ_COMPANY_FULL_NAME",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 530
        },
        {
            "ID": 4,
            "FIELD_NAME": "RQ_OGRN",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 540
        },
        {
            "ID": 5,
            "FIELD_NAME": "RQ_KPP",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "Y",
            "SORT": 550
        },
        {
            "ID": 6,
            "FIELD_NAME": "RQ_COMPANY_REG_DATE",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 560
        },
        {
            "ID": 7,
            "FIELD_NAME": "RQ_OKPO",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 570
        },
        {
            "ID": 8,
            "FIELD_NAME": "RQ_OKTMO",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 580
        },
        {
            "ID": 9,
            "FIELD_NAME": "RQ_DIRECTOR",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 590
        },
        {
            "ID": 10,
            "FIELD_NAME": "RQ_ACCOUNTANT",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 600
        },
        {
            "ID": 11,
            "FIELD_NAME": "RQ_ADDR",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 610
        },
        {
            "ID": 12,
            "FIELD_NAME": "RQ_SIGNATURE",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 620
        },
        {
            "ID": 13,
            "FIELD_NAME": "RQ_STAMP",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "N",
            "SORT": 630
        },
        {
            "ID": 14,
            "FIELD_NAME": "UF_CRM_1703689889",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "Y",
            "SORT": 640
        },
        {
            "ID": 15,
            "FIELD_NAME": "UF_CRM_1703690003",
            "FIELD_TITLE": "",
            "IN_SHORT_LIST": "Y",
            "SORT": 650
        }
    ],
    "time": {
        "start": 1716895759.934609,
        "finish": 1716895760.407579,
        "duration": 0.47297000885009766,
        "processing": 0.023286104202270508,
        "date_start": "2024-05-28T13:29:19+02:00",
        "date_finish": "2024-05-28T13:29:20+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../../data-types.md)| An array of objects describing the customizable fields of the requisites template. Each element contains [fields](#fields) of the customizable field template ||
|| **total**
[`integer`](../../../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../../../data-types.md) | Information about the request execution time ||
|#

### Fields Describing The {#fields}

Customizable Field of the Requisites Template
#|
||  **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../../data-types.md) | Field identifier. Created automatically and unique within the template ||
|| **FIELD_NAME**
[`string`](../../../../data-types.md) | Field name ||
|| **FIELD_TITLE**
[`string`](../../../../data-types.md) | An alternative name for the field in the requisite.

The alternative name is displayed in various forms for filling out requisites. Depending on the specific form, the alternative name may or may not be used 
||
|| **SORT**
[`integer`](../../../../data-types.md) | Sorting. The order in the list of template fields || 
|| **IN_SHORT_LIST**
[`char`](../../../../data-types.md) | Show in the short list. Deprecated field, currently not used. Retained for backward compatibility. Can take values `Y` or `N` ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "The Preset with ID '27' is not found"
}
```

{% include notitle [Error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `The Preset with ID '27' is not found` | Template with the specified identifier not found ||
|| `Access denied` | Insufficient access permissions to retrieve the list of customizable fields of the template ||
|#

{% include [System errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-preset-field-add.md)
- [{#T}](./crm-requisite-preset-field-update.md)
- [{#T}](./crm-requisite-preset-field-available-to-add.md)
- [{#T}](./crm-requisite-preset-field-get.md)
- [{#T}](./crm-requisite-preset-field-delete.md)
- [{#T}](./crm-requisite-preset-field-fields.md)