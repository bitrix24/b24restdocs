# Set Localizations for Currency crm.currency.localizations.set

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to modify CRM settings

This method updates localizations for a currency or adds them if the localization for the specified language does not exist.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
||  **Name** /
`type`| **Description** ||
|| **id***
[`string`](../../../data-types.md) | Currency identifier.

Corresponds to the ISO 4217 standard.

The identifier can be obtained using the [crm.currency.list](../crm-currency-list.md) method
 ||
|| **localizations***
[`object`](../../../data-types.md) | Currency localization parameters.
An object in the format `{"lang_1": "value_1", ... "lang_N": "value_N"}`, where `lang_N` is the language identifier for which to add/change the localization, and `value` is an object of type [crm_currency_localization](../../data-types.md#crm_currency_localization).

Existing localizations that are not passed to the method will not be changed.
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
    -d '{"id":"CLF","localizations":{"en":{"FULL_NAME":"Unidad de Fomento","FORMAT_STRING":"CLF#VALUE#","DEC_POINT":".","THOUSANDS_VARIANT":"C","DECIMALS":4},"de":{"FULL_NAME":"Einheit der Entwicklung","FORMAT_STRING":"#VALUE# CLF","DEC_POINT":".","THOUSANDS_VARIANT":"B","DECIMALS":4}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.currency.localizations.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"CLF","localizations":{"en":{"FULL_NAME":"Unidad de Fomento","FORMAT_STRING":"CLF#VALUE#","DEC_POINT":".","THOUSANDS_VARIANT":"C","DECIMALS":4},"de":{"FULL_NAME":"Einheit der Entwicklung","FORMAT_STRING":"#VALUE# CLF","DEC_POINT":".","THOUSANDS_VARIANT":"B","DECIMALS":4}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.currency.localizations.set
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
        method: 'crm.currency.localizations.set',
        params: {
          id: 'CLF',
          localizations: {
            en: {
              FULL_NAME: 'Unidad de Fomento',
              FORMAT_STRING: 'CLF#VALUE#',
              DEC_POINT: '.',
              THOUSANDS_VARIANT: 'C',
              DECIMALS: 4,
            },
            ru: {
              FULL_NAME: 'Unit of Account',
              FORMAT_STRING: '#VALUE# CLF',
              DEC_POINT: '.',
              THOUSANDS_VARIANT: 'B',
              DECIMALS: 4,
            },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Localizations set:', result)
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
      async function setCurrencyLocalizations() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.currency.localizations.set',
            params: {
              id: 'CLF',
              localizations: {
                en: {
                  FULL_NAME: 'Unidad de Fomento',
                  FORMAT_STRING: 'CLF#VALUE#',
                  DEC_POINT: '.',
                  THOUSANDS_VARIANT: 'C',
                  DECIMALS: 4,
                },
                ru: {
                  FULL_NAME: 'Unit of Account',
                  FORMAT_STRING: '#VALUE# CLF',
                  DEC_POINT: '.',
                  THOUSANDS_VARIANT: 'B',
                  DECIMALS: 4,
                },
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
          console.info('Localizations set:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setCurrencyLocalizations)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.currency.localizations.set',
                [
                    'id' => 'CLF',
                    'localizations' => [
                        'en' => [
                            'FULL_NAME'        => 'Unidad de Fomento',
                            'FORMAT_STRING'    => 'CLF#VALUE#',
                            'DEC_POINT'        => '.',
                            'THOUSANDS_VARIANT' => 'C',
                            'DECIMALS'         => 4,
                        ],
                        'de' => [
                            'FULL_NAME'        => 'Einheit der Entwicklung',
                            'FORMAT_STRING'    => '#VALUE# CLF',
                            'DEC_POINT'        => '.',
                            'THOUSANDS_VARIANT' => 'B',
                            'DECIMALS'         => 4,
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting currency localizations: ' . $e->getMessage();
    }
    ```

- Python

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.currency.localizations.set(
            bitrix_id="CLF",
            localizations={
                "en": {
                    "FULL_NAME": "Unidad de Fomento",
                    "FORMAT_STRING": "CLF#VALUE#",
                    "DEC_POINT": ".",
                    "THOUSANDS_VARIANT": "C",
                    "DECIMALS": 4,
                },
                "de": {
                    "FULL_NAME": "Einheit der Entwicklung",
                    "FORMAT_STRING": "#VALUE# CLF",
                    "DEC_POINT": ".",
                    "THOUSANDS_VARIANT": "B",
                    "DECIMALS": 4,
                },
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

- BX24.js

    ```js
    BX24.callMethod(
        "crm.currency.localizations.set",
        {
            id: 'CLF',
            localizations: {
                en: {
                    FULL_NAME: 'Unidad de Fomento',
                    FORMAT_STRING: 'CLF#VALUE#',
                    DEC_POINT: '.',
                    THOUSANDS_VARIANT: 'C',
                    DECIMALS: 4,
                },
                ru: {
                    FULL_NAME: 'Einheit der Entwicklung',
                    FORMAT_STRING: '#VALUE

# CLF',
                    DEC_POINT: '.',
                    THOUSANDS_VARIANT: 'B',
                    DECIMALS: 4,
                }
            }
        },
    )
    .then(
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result);
            }
        },
        function(error)
        {
            console.info(error);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.currency.localizations.set',
        [
            'id' => 'CLF',
            'localizations' => [
                'en' => [
                    'FULL_NAME' => 'Unidad de Fomento',
                    'FORMAT_STRING' => 'CLF#VALUE#',
                    'DEC_POINT' => '.',
                    'THOUSANDS_VARIANT' => 'C',
                    'DECIMALS' => 4,
                ],
                'de' => [
                    'FULL_NAME' => 'Einheit der Entwicklung',
                    'FORMAT_STRING' => '#VALUE

# CLF',
                    'DEC_POINT' => '.',
                    'THOUSANDS_VARIANT' => 'B',
                    'DECIMALS' => 4,
                ]
            ]
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
    "result": true,
    "time": {
        "start": 1718122481.837301,
        "finish": 1718122483.141736,
        "duration": 1.3044350147247314,
        "processing": 0.08866286277770996,
        "date_start": "2024-06-11T18:14:41+02:00",
        "date_finish": "2024-06-11T18:14:43+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Returns:
- `true` — on success
- `false` – if the operation could not be completed, but there is no error, or the situation is not considered erroneous. Possible scenarios:
  - currency module is missing
  - an empty object with localizations was passed
  - no localization was added/changed
 ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "The parameter id is invalid or not defined."
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty string | Access denied. | Insufficient access rights. ||
|| Empty string | The parameter id is invalid or not defined. | Empty currency identifier ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-currency-localizations-get.md)
- [{#T}](./crm-currency-localizations-delete.md)
- [{#T}](./crm-currency-localizations-fields.md)