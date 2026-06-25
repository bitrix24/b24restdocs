# Update Currency crm.currency.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user with access to modify CRM settings

This method updates an existing currency.

## Parameters

#|
||  **Name**
`type`| **Description** ||
|| **ID**
[`string`](../../data-types.md) | Currency identifier. 

Corresponds to the ISO 4217 standard.

The identifier can be obtained using the [crm.currency.list](./crm-currency-list.md) method.
 ||
|| **fields**
[`object`](../../data-types.md) | Field values (detailed description provided [below](#parametr-fields)) for updating the currency in the following structure:

```js
fields: {
    SORT: 'value'
    AMOUNT_CNT: 'value',
    AMOUNT: 'value'
    BASE: 'value',
    LANG: {
        lang_1: {
            DECIMALS: 'value',
            DEC_POINT: 'value',
            ...
        },
        ...
        lang_N: {
            ...
        }
    }
}
```
||
|#

### Parameter fields

{% include [Note on parameters](../../../_includes/required.md) %}

#|
||  **Name**
`type`| **Description** ||
|| **SORT**
[`integer`](../../data-types.md) | Position in the currency list.

 Default value is `100`.
 ||
|| **AMOUNT_CNT***
[`integer`](../../data-types.md) | Denomination. Typically, `1` or a multiple of `10` is used as the denomination.
 ||
|| **AMOUNT***
[`double`](../../data-types.md) | Exchange rate relative to the base currency. ||
|| **BASE**
[`string`](../../data-types.md) | Indicates whether the currency is a base currency.

Possible values:
 - `Y` — yes
 - `N` — no

Default value is `N`.

{% note warning %}

It is not recommended to change the base currency via REST. Otherwise, you will need to change the rates of all currencies.

{% endnote %}

 ||
|| **LANG**
[`object`](../../data-types.md) | Currency localization parameters.

An object in the format `{"lang_1": "value_1", ... "lang_N": "value_N"}`, where `lang_N` is the language identifier, and `value` is an object of type [crm_currency_localization](../data-types.md#crm_currency_localization).

If not all languages are specified, [default localization parameters](../data-types.md#crm_currency_localization) will be used for the remaining ones.
 ||
|#

### Parameter LANG

#|
||  **Name**
`type`| **Description** ||
|| **DECIMALS***
[`int`](../../data-types.md) | Number of decimal places in the fractional part. ||
|| **DEC_POINT**
[`string`](../../data-types.md) | Decimal point for output. ||
|| **FORMAT_STRING**
[`string`](../../data-types.md) | Format template. ||
|| **FULL_NAME**
[`string`](../../data-types.md) | Name of the currency in the language for which localization is added. ||
|| **HIDE_ZERO**
[`string`](../../data-types.md) | Indicates whether to hide insignificant zeros. ||
|| **THOUSANDS_SEP**
[`string`](../../data-types.md) | Thousands separator. ||
|| **THOUSANDS_VARIANT**
[`string`](../../data-types.md) | Code for the thousands separator.

Refer to the [crm_currency_localization](../data-types.md#crm_currency_localization) documentation for acceptable values. ||
|#

## Examples

1. Changing the exchange rate of the yuan relative to the base currency.

    {% include [Note on examples](../../../_includes/examples.md) %}

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"ID":"CNY","fields":{"AMOUNT":15.3449}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.currency.update
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"ID":"CNY","fields":{"AMOUNT":15.3449},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.currency.update
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
            method: 'crm.currency.update',
            params: {
              ID: 'CNY',
              fields: {
                AMOUNT: 15.3449,
              },
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info('Currency updated:', result)
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
          async function updateCurrency() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const response = await $b24.actions.v2.call.make({
                method: 'crm.currency.update',
                params: {
                  ID: 'CNY',
                  fields: {
                    AMOUNT: 15.3449,
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
              console.info('Currency updated:', result)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', updateCurrency)
        </script>
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.currency.update',
            [
                'ID' => 'CNY',
                'fields' => [
                    'AMOUNT' => 15.3449,
                ]
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    - Python

        ```python
        from b24pysdk.client import BaseClient
        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        client: BaseClient

        try:
            bitrix_response = client.crm.currency.update(
                bitrix_id="CNY",
                fields={
                    "AMOUNT": 15.3449,
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

    {% endlist %}

2. Changing the localizations of the currency (using the example of the US dollar)

    After executing this example, the localizations for the currency USD will be changed (or added) only for English and German languages. Localizations for other languages, if they existed, will not be changed.

    This example is similar to the operation of the [crm.currency.localizations.set](./localizations/crm-currency-localizations-set.md) method.

    {% include [Note on examples](../../../_includes/examples.md) %}

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"ID":"USD","fields":{"LANG":{"en":{"DECIMALS":2,"DEC_POINT":".","FORMAT_STRING":"$#","FULL_NAME":"US Dollar","HIDE_ZERO":"Y","THOUSANDS_VARIANT":"S"},"de":{"DECIMALS":2,"DEC_POINT":".","FORMAT_STRING":"# $","FULL_NAME":"US-Dollar","HIDE_ZERO":"Y","THOUSANDS_VARIANT":"C"}}}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.currency.update
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"ID":"USD","fields":{"LANG":{"en":{"DECIMALS":2,"DEC_POINT":".","FORMAT_STRING":"$#","FULL_NAME":"US Dollar","HIDE_ZERO":"Y","THOUSANDS_VARIANT":"S"},"de":{"DECIMALS":2,"DEC_POINT":".","FORMAT_STRING":"# $","FULL_NAME":"US-Dollar","HIDE_ZERO":"Y","THOUSANDS_VARIANT":"C"}}},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.currency.update
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
            method: 'crm.currency.update',
            params: {
              ID: 'USD',
              fields: {
                LANG: {
                  en: {
                    DECIMALS: 2,
                    DEC_POINT: '.',
                    FORMAT_STRING: '$#',
                    FULL_NAME: 'US Dollar',
                    HIDE_ZERO: 'Y',
                    THOUSANDS_VARIANT: 'S',
                  },
                  de: {
                    DECIMALS: 2,
                    DEC_POINT: '.',
                    FORMAT_STRING: '# $',
                    FULL_NAME: 'US-Dollar',
                    HIDE_ZERO: 'Y',
                    THOUSANDS_VARIANT: 'C',
                  },
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
            console.info('Currency updated:', result)
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
          async function updateCurrency() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const response = await $b24.actions.v2.call.make({
                method: 'crm.currency.update',
                params: {
                  ID: 'USD',
                  fields: {
                    LANG: {
                      en: {
                        DECIMALS: 2,
                        DEC_POINT: '.',
                        FORMAT_STRING: '$#',
                        FULL_NAME: 'US Dollar',
                        HIDE_ZERO: 'Y',
                        THOUSANDS_VARIANT: 'S',
                      },
                      de: {
                        DECIMALS: 2,
                        DEC_POINT: '.',
                        FORMAT_STRING: '# $',
                        FULL_NAME: 'US-Dollar',
                        HIDE_ZERO: 'Y',
                        THOUSANDS_VARIANT: 'C',
                      },
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
              console.info('Currency updated:', result)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', updateCurrency)
        </script>
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.currency.update',
            [
                'ID' => 'USD',
                'fields' => [
                    'LANG' => [
                        'en' => [
                            'DECIMALS' => 2,
                            'DEC_POINT' => '.',
                            'FORMAT_STRING' => '$#',
                            'FULL_NAME' => 'US Dollar',
                            'HIDE_ZERO' => 'Y',
                            'THOUSANDS_VARIANT' => 'S',
                        ],
                        'de' => [
                            'DECIMALS' => 2,
                            'DEC_POINT' => '.',
                            'FORMAT_STRING' => '# $',
                            'FULL_NAME' => 'US-Dollar',
                            'HIDE_ZERO' => 'Y',
                            'THOUSANDS_VARIANT' => 'C',
                        ]
                    ]
                ]
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    - Python

        ```python
        from b24pysdk.client import BaseClient
        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        client: BaseClient

        try:
            bitrix_response = client.crm.currency.update(
                bitrix_id="USD",
                fields={
                    "LANG": {
                        "en": {
                            "DECIMALS": 2,
                            "DEC_POINT": ".",
                            "FORMAT_STRING": "$#",
                            "FULL_NAME": "US Dollar",
                            "HIDE_ZERO": "Y",
                            "THOUSANDS_VARIANT": "S",
                        },
                        "de": {
                            "DECIMALS": 2,
                            "DEC_POINT": ".",
                            "FORMAT_STRING": '# $',
                            "FULL_NAME": "US-Dollar",
                            "HIDE_ZERO": "Y",
                            "THOUSANDS_VARIANT": "C",
                        },
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

    {% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1717764521.938284,
        "finish": 1717764522.516576,
        "duration": 0.5782921314239502,
        "processing": 0.07656002044677734,
        "date_start": "2024-06-07T14:48:41+02:00",
        "date_finish": "2024-06-07T14:48:42+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the currency update. ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
	"error": "",
	"error_description": "Access denied."
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty string | Access denied. | Insufficient access rights. ||
|| Empty string | "Currencies" module not found! Please install the "Currencies" module. |  ||
|| `ERROR_CODE` | Other errors in the data for changing the currency |  ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-currency-add.md)
- [{#T}](./crm-currency-get.md)
- [{#T}](./crm-currency-list.md)
- [{#T}](./crm-currency-delete.md)
- [{#T}](./crm-currency-fields.md)