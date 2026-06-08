# Add Currency crm.currency.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user with access to modify CRM settings

This method adds a new currency.

For the languages used on the account, localization parameters must be specified. If not provided, [default parameters](../data-types.md#crm_currency_localization) will be used. Localization parameters for a specific language can be set using the [crm.currency.localizations.set](./localizations/crm-currency-localizations-set.md) method.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
||  **Name**
`type`| **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values (detailed description provided [below](#parametr-fields)) for adding a new currency in the form of a structure:

```js
fields: {
    CURRENCY: 'value',
    BASE: 'value',
    AMOUNT_CNT: 'value',
    AMOUNT: 'value'
    SORT: 'value'
    LANG: 'value'
}
```

||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
||  **Name**
`type`| **Description** ||
|| **CURRENCY***
[`string`](../../data-types.md) | Currency identifier.

Corresponds to the ISO 4217 standard
 ||
|| **BASE**
[`string`](../../data-types.md) | Indicates whether the currency is base.

Possible values:
 - `Y` — yes
 - `N` — no

Default value is `N`.

{% note warning %}

It is not recommended to change the base currency via REST. Otherwise, it will be necessary to change the rates of all currencies.

{% endnote %}

 ||
|| **AMOUNT_CNT***
[`int`](../../data-types.md) | Denomination. Typically, `1` or a multiple of `10` is used as the denomination.
 ||
|| **AMOUNT***
[`double`](../../data-types.md) | Exchange rate relative to the base currency. ||
|| **SORT**
[`int`](../../data-types.md) | Position in the currency list.

 Default value is `100`.
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

Allowed values are described in the [reference](../data-types.md#crm_currency_localization). ||
|#

## Code Examples

1. Creating the yuan with localization parameters (Russian and English languages)

    The base currency is the ruble. The exchange rate for the yuan is 12.2251 rubles for 1 yuan.

    {% include [Note on examples](../../../_includes/examples.md) %}

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"CURRENCY":"CNY","BASE":"N","AMOUNT":12.2251,"AMOUNT_CNT":1,"SORT":9000,"LANG":{"ru":{"DECIMALS":2,"DEC_POINT":".","FORMAT_STRING":"# CNY","FULL_NAME":"yuan","HIDE_ZERO":"Y","THOUSANDS_VARIANT":"S"},"en":{"DECIMALS":2,"DEC_POINT":",","FORMAT_STRING":"# CNY","FULL_NAME":"yuan","HIDE_ZERO":"Y","THOUSANDS_SEP":"."}}}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.currency.add
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"CURRENCY":"CNY","BASE":"N","AMOUNT":12.2251,"AMOUNT_CNT":1,"SORT":9000,"LANG":{"ru":{"DECIMALS":2,"DEC_POINT":".","FORMAT_STRING":"# CNY","FULL_NAME":"yuan","HIDE_ZERO":"Y","THOUSANDS_VARIANT":"S"},"en":{"DECIMALS":2,"DEC_POINT":",","FORMAT_STRING":"# CNY","FULL_NAME":"yuan","HIDE_ZERO":"Y","THOUSANDS_SEP":"."}}},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.currency.add
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        try {
          const response = await $b24.actions.v2.call.make<string>({
            method: 'crm.currency.add',
            params: {
              fields: {
                CURRENCY: 'CNY',
                BASE: 'N',
                AMOUNT: 12.2251,
                AMOUNT_CNT: 1,
                SORT: 9000,
                LANG: {
                  ru: {
                    DECIMALS: 2,
                    DEC_POINT: '.',
                    FORMAT_STRING: '# CNY',
                    FULL_NAME: 'Yuan',
                    HIDE_ZERO: 'Y',
                    THOUSANDS_VARIANT: 'S',
                  },
                  en: {
                    DECIMALS: 2,
                    DEC_POINT: ',',
                    FORMAT_STRING: '# CNY',
                    FULL_NAME: 'Yuan',
                    HIDE_ZERO: 'Y',
                    THOUSANDS_SEP: '.',
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
            console.info('Created currency id:', result)
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
          async function addCurrency() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const response = await $b24.actions.v2.call.make({
                method: 'crm.currency.add',
                params: {
                  fields: {
                    CURRENCY: 'CNY',
                    BASE: 'N',
                    AMOUNT: 12.2251,
                    AMOUNT_CNT: 1,
                    SORT: 9000,
                    LANG: {
                      ru: {
                        DECIMALS: 2,
                        DEC_POINT: '.',
                        FORMAT_STRING: '# CNY',
                        FULL_NAME: 'Yuan',
                        HIDE_ZERO: 'Y',
                        THOUSANDS_VARIANT: 'S',
                      },
                      en: {
                        DECIMALS: 2,
                        DEC_POINT: ',',
                        FORMAT_STRING: '# CNY',
                        FULL_NAME: 'Yuan',
                        HIDE_ZERO: 'Y',
                        THOUSANDS_SEP: '.',
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
              console.info('Created currency id:', result)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', addCurrency)
        </script>
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.currency.add',
            [
                'fields' => [
                    'CURRENCY' => 'CNY',
                    'BASE' => 'N',
                    'AMOUNT' => 12.2251,
                    'AMOUNT_CNT' => 1,
                    'SORT' => 9000,
                    'LANG' => [
                        'ru' => [
                            'DECIMALS' => 2,
                            'DEC_POINT' => '.',
                            'FORMAT_STRING' => '# CNY',
                            'FULL_NAME' => 'yuan',
                            'HIDE_ZERO' => 'Y',
                            'THOUSANDS_VARIANT' => 'S',
                        ],
                        'en' => [
                            'DECIMALS' => 2,
                            'DEC_POINT' => ',',
                            'FORMAT_STRING' => '# CNY',
                            'FULL_NAME' => 'yuan',
                            'HIDE_ZERO' => 'Y',
                            'THOUSANDS_SEP' => '.',
                        ],
                    ],
                ]
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}

2. Creating the Indonesian rupiah

    Assuming the base currency is the ruble. The currency has a very low exchange rate — 1 rupiah is equivalent to 0.00548 rubles. In this case, we increase the denomination (AMOUNT_CNT) to set the rate with the required precision.

    {% include [Note on examples](../../../_includes/examples.md) %}

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"CURRENCY":"IDR","AMOUNT":54.8738,"AMOUNT_CNT":10000,"SORT":8000,"LANG":{"ru":{"DECIMALS":2,"DEC_POINT":".","FORMAT_STRING":"Rp#","FULL_NAME":"rupiah","HIDE_ZERO":"Y","THOUSANDS_VARIANT":"C"},"en":{"DECIMALS":2,"DEC_POINT":".","FORMAT_STRING":"# CNY","FULL_NAME":"rupee","HIDE_ZERO":"Y","THOUSANDS_VARIANT":"C"}}}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.currency.add
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"CURRENCY":"IDR","AMOUNT":54.8738,"AMOUNT_CNT":10000,"SORT":8000,"LANG":{"ru":{"DECIMALS":2,"DEC_POINT":".","FORMAT_STRING":"Rp#","FULL_NAME":"rupiah","HIDE_ZERO":"Y","THOUSANDS_VARIANT":"C"},"en":{"DECIMALS":2,"DEC_POINT":".","FORMAT_STRING":"# CNY","FULL_NAME":"rupee","HIDE_ZERO":"Y","THOUSANDS_VARIANT":"C"}}},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.currency.add
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        try {
          const response = await $b24.actions.v2.call.make<string>({
            method: 'crm.currency.add',
            params: {
              fields: {
                CURRENCY: 'IDR',
                AMOUNT: 54.8738,
                AMOUNT_CNT: 10000,
                SORT: 8000,
                LANG: {
                  ru: {
                    DECIMALS: 2,
                    DEC_POINT: '.',
                    FORMAT_STRING: 'Rp#',
                    FULL_NAME: 'Rupee',
                    HIDE_ZERO: 'Y',
                    THOUSANDS_VARIANT: 'C',
                  },
                  en: {
                    DECIMALS: 2,
                    DEC_POINT: '.',
                    FORMAT_STRING: 'Rp#',
                    FULL_NAME: 'Rupee',
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
            console.info('Created currency id:', result)
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
          async function addCurrency() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const response = await $b24.actions.v2.call.make({
                method: 'crm.currency.add',
                params: {
                  fields: {
                    CURRENCY: 'IDR',
                    AMOUNT: 54.8738,
                    AMOUNT_CNT: 10000,
                    SORT: 8000,
                    LANG: {
                      ru: {
                        DECIMALS: 2,
                        DEC_POINT: '.',
                        FORMAT_STRING: 'Rp#',
                        FULL_NAME: 'Rupee',
                        HIDE_ZERO: 'Y',
                        THOUSANDS_VARIANT: 'C',
                      },
                      en: {
                        DECIMALS: 2,
                        DEC_POINT: '.',
                        FORMAT_STRING: 'Rp#',
                        FULL_NAME: 'Rupee',
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
              console.info('Created currency id:', result)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', addCurrency)
        </script>
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.currency.add',
            [
                'fields' => [
                    'CURRENCY' => 'IDR',
                    'AMOUNT' => 54.8738,
                    'AMOUNT_CNT' => 10000,
                    'SORT' => 8000,
                    'LANG' => [
                        'ru' => [
                            'DECIMALS' => 2,
                            'DEC_POINT' => '.',
                            'FORMAT_STRING' => 'Rp#',
                            'FULL_NAME' => 'rupiah',
                            'HIDE_ZERO' => 'Y',
                            'THOUSANDS_VARIANT' => 'C',
                        ],
                        'en' => [
                            'DECIMALS' => 2,
                            'DEC_POINT' => '.',
                            'FORMAT_STRING' => '# CNY',
                            'FULL_NAME' => 'rupee',
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

    {% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": "CNY",
    "time": {
        "start": 1717684463.467685,
        "finish": 1717684465.282238,
        "duration": 1.8145530223846436,
        "processing": 0.12580394744873047,
        "date_start": "2024-06-06T16:34:23+02:00",
        "date_finish": "2024-06-06T16:34:25+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`crm_currency.CURRENCY`](../data-types.md#crm_currency) | Identifier of the created currency. ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time. ||
|#

## Error Handling

HTTP status: **400**

```json
{
	"error": "",
	"error_description": "Access denied."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty string | Access denied. | Insufficient access rights. ||
|| Empty string | The "Currency" module is not found! Please install the "Currency" module. |  ||
|| `ERROR_CORE` | Undefined array key "#FIELD#" | Required field #FIELD# is not specified (the missing field code will be substituted instead of #FIELD#) ||
|| `ERROR_CORE` | Other errors in the data for creating the currency. |  ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-currency-update.md)
- [{#T}](./crm-currency-get.md)
- [{#T}](./crm-currency-list.md)
- [{#T}](./crm-currency-delete.md)
- [{#T}](./crm-currency-fields.md)