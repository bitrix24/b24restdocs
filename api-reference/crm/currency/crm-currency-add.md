# Add Currency crm.currency.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Edit CRM preferences" permission in CRM

The `crm.currency.add` method adds a currency and returns its code.

Pass the localization parameters — the currency name and the amount format for each Bitrix24 language — in the `LANG` field. Languages missing from it get the [default parameters](../data-types.md#crm_currency_localization). You can also set the parameters for a single language separately with the [crm.currency.localizations.set](./localizations/crm-currency-localizations-set.md) method.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type`| **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values (detailed description provided [below](#parameter-fields)) for adding a new currency in the form of a structure:

```js
fields: {
    CURRENCY: 'value',
    BASE: 'value',
    AMOUNT_CNT: 'value',
    AMOUNT: 'value',
    SORT: 'value',
    LANG: 'value'
}
```

||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type`| **Description** ||
|| **CURRENCY***
[`string`](../../data-types.md) | Currency identifier.

Corresponds to the ISO 4217 standard
 ||
|| **BASE**
[`string`](../../data-types.md) | Base currency flag. The method ignores this parameter and does not return an error: you cannot make a currency the base currency with this method. The base currency is changed by the [crm.currency.base.set](./crm-currency-base-set.md) method
 ||
|| **AMOUNT_CNT***
[`integer`](../../data-types.md) | Denomination — the number of currency units for which the `AMOUNT` rate is set. Usually `1`, `10`, `100`, and so on
 ||
|| **AMOUNT***
[`double`](../../data-types.md) | The value of the `AMOUNT_CNT` denomination in the base currency, for example `0.1220` ||
|| **SORT**
[`integer`](../../data-types.md) | Position in the currency list.

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
|| **Name**
`type`| **Description** ||
|| **DECIMALS**
[`integer`](../../data-types.md) | Number of decimal places ||
|| **DEC_POINT**
[`string`](../../data-types.md) | Decimal separator ||
|| **FORMAT_STRING***
[`string`](../../data-types.md) | Amount display template, for example `# CNY` ||
|| **FULL_NAME**
[`string`](../../data-types.md) | Name of the currency in the language for which localization is added. ||
|| **HIDE_ZERO**
[`string`](../../data-types.md) | Whether to hide insignificant zeros: `Y` — hide, `N` — show. The method saves any other value as `N` ||
|| **THOUSANDS_SEP**
[`string`](../../data-types.md) | Thousands separator ||
|| **THOUSANDS_VARIANT**
[`string`](../../data-types.md) | Code for the thousands separator.

Allowed values are described in the [reference](../data-types.md#crm_currency_localization). ||
|#

## Code Examples

1. Creating the yuan with localization parameters (Russian and English languages)

    The base currency is the euro. One yuan costs 0.1220 euros.

    {% include [Note on examples](../../../_includes/examples.md) %}

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"CURRENCY":"CNY","BASE":"N","AMOUNT":0.1220,"AMOUNT_CNT":1,"SORT":9000,"LANG":{"ru":{"DECIMALS":2,"DEC_POINT":".","FORMAT_STRING":"# CNY","FULL_NAME":"yuan","HIDE_ZERO":"Y","THOUSANDS_VARIANT":"S"},"en":{"DECIMALS":2,"DEC_POINT":",","FORMAT_STRING":"# CNY","FULL_NAME":"yuan","HIDE_ZERO":"Y","THOUSANDS_VARIANT":"D"}}}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.currency.add
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"CURRENCY":"CNY","BASE":"N","AMOUNT":0.1220,"AMOUNT_CNT":1,"SORT":9000,"LANG":{"ru":{"DECIMALS":2,"DEC_POINT":".","FORMAT_STRING":"# CNY","FULL_NAME":"yuan","HIDE_ZERO":"Y","THOUSANDS_VARIANT":"S"},"en":{"DECIMALS":2,"DEC_POINT":",","FORMAT_STRING":"# CNY","FULL_NAME":"yuan","HIDE_ZERO":"Y","THOUSANDS_VARIANT":"D"}}},"auth":"**put_access_token_here**"}' \
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
                AMOUNT: 0.1220,
                AMOUNT_CNT: 1,
                SORT: 9000,
                LANG: {
                  ru: {
                    DECIMALS: 2,
                    DEC_POINT: '.',
                    FORMAT_STRING: '# CNY',
                    FULL_NAME: 'yuan',
                    HIDE_ZERO: 'Y',
                    THOUSANDS_VARIANT: 'S',
                  },
                  en: {
                    DECIMALS: 2,
                    DEC_POINT: ',',
                    FORMAT_STRING: '# CNY',
                    FULL_NAME: 'yuan',
                    HIDE_ZERO: 'Y',
                    THOUSANDS_VARIANT: 'D',
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
                    AMOUNT: 0.1220,
                    AMOUNT_CNT: 1,
                    SORT: 9000,
                    LANG: {
                      ru: {
                        DECIMALS: 2,
                        DEC_POINT: '.',
                        FORMAT_STRING: '# CNY',
                        FULL_NAME: 'yuan',
                        HIDE_ZERO: 'Y',
                        THOUSANDS_VARIANT: 'S',
                      },
                      en: {
                        DECIMALS: 2,
                        DEC_POINT: ',',
                        FORMAT_STRING: '# CNY',
                        FULL_NAME: 'yuan',
                        HIDE_ZERO: 'Y',
                        THOUSANDS_VARIANT: 'D',
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

    - Python

        ```python

        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        try:
            bitrix_response = client.crm.currency.add(
                fields={
                    "CURRENCY": "CNY",
                    "BASE": "N",
                    "AMOUNT": 0.1220,
                    "AMOUNT_CNT": 1,
                    "SORT": 9000,
                    "LANG": {
                        "de": {
                            "DECIMALS": 2,
                            "DEC_POINT": ".",
                            "FORMAT_STRING": "# CNY",
                            "FULL_NAME": "yuan",
                            "HIDE_ZERO": "Y",
                            "THOUSANDS_VARIANT": "S",
                        },
                        "en": {
                            "DECIMALS": 2,
                            "DEC_POINT": ",",
                            "FORMAT_STRING": "# CNY",
                            "FULL_NAME": "yuan",
                            "HIDE_ZERO": "Y",
                            "THOUSANDS_VARIANT": "D",
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

    - PHP

        ```php
        try {
            $response = $b24Service
                ->core
                ->call(
                    'crm.currency.add',
                    [
                        'fields' => [
                            'CURRENCY' => 'CNY',
                            'BASE' => 'N',
                            'AMOUNT' => 0.1220,
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
                                    'THOUSANDS_VARIANT' => 'D',
                                ],
                            ],
                        ],
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
            "crm.currency.add",
            {
                fields: {
                    CURRENCY: 'CNY',
                    BASE: 'N',
                    AMOUNT: 0.1220,
                    AMOUNT_CNT: 1,
                    SORT: 9000,
                    LANG: {
                        ru: {
                            DECIMALS: 2,
                            DEC_POINT: '.',
                            FORMAT_STRING: '# CNY',
                            FULL_NAME: 'yuan',
                            HIDE_ZERO: 'Y',
                            THOUSANDS_VARIANT: 'S'
                        },
                        en: {
                            DECIMALS: 2,
                            DEC_POINT: ',',
                            FORMAT_STRING: '# CNY',
                            FULL_NAME: 'yuan',
                            HIDE_ZERO: 'Y',
                            THOUSANDS_VARIANT: 'D'
                        }
                    }
                }
            },
            function(result)
            {
                if (result.error())
                {
                    console.error(result.error());
                }
                else
                {
                    console.log(result.data());
                }
            }
        );
        ```

    - PHP CRest

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.currency.add',
            [
                'fields' => [
                    'CURRENCY' => 'CNY',
                    'BASE' => 'N',
                    'AMOUNT' => 0.1220,
                    'AMOUNT_CNT' => 1,
                    'SORT' => 9000,
                    'LANG' => [
                        'de' => [
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
                            'THOUSANDS_VARIANT' => 'D',
                        ],
                    ],
                ]
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    - Go

        ```go
        // client and ctx are already created — see the Go SDK section
        res, err := client.Core().Call(ctx, "crm.currency.add", b24.Params{
        	"fields": b24.Params{
        		"CURRENCY":   "CNY",
        		"BASE":       "N",
        		"AMOUNT":     0.1220,
        		"AMOUNT_CNT": 1,
        		"SORT":       9000,
        		"LANG": b24.Params{
        			"ru": b24.Params{
        				"DECIMALS":          2,
        				"DEC_POINT":         ".",
        				"FORMAT_STRING":     "# CNY",
        				"FULL_NAME":         "yuan",
        				"HIDE_ZERO":         "Y",
        				"THOUSANDS_VARIANT": "S",
        			},
        			"en": b24.Params{
        				"DECIMALS":      2,
        				"DEC_POINT":     ",",
        				"FORMAT_STRING": "# CNY",
        				"FULL_NAME":     "yuan",
        				"HIDE_ZERO":     "Y",
        				"THOUSANDS_VARIANT": "D",
        			},
        		},
        	},
        })
        if err != nil {
        	return fmt.Errorf("crm.currency.add: %w", err)
        }

        var value string
        if err := json.Unmarshal(res.Result, &value); err != nil {
        	return fmt.Errorf("parse response: %w", err)
        }
        fmt.Println("result:", value)
        ```

    {% endlist %}

2. Creating the Indonesian rupiah

    The base currency is the euro. One rupiah costs 0.00005405 euros. To set such a low rate precisely, we increase the denomination: `AMOUNT` = `0.5405` euros per `AMOUNT_CNT` = `10000` rupiah.

    {% include [Note on examples](../../../_includes/examples.md) %}

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"CURRENCY":"IDR","AMOUNT":0.5405,"AMOUNT_CNT":10000,"SORT":8000,"LANG":{"ru":{"DECIMALS":2,"DEC_POINT":".","FORMAT_STRING":"Rp#","FULL_NAME":"rupiah","HIDE_ZERO":"Y","THOUSANDS_VARIANT":"C"},"en":{"DECIMALS":2,"DEC_POINT":".","FORMAT_STRING":"Rp#","FULL_NAME":"rupee","HIDE_ZERO":"Y","THOUSANDS_VARIANT":"C"}}}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.currency.add
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"CURRENCY":"IDR","AMOUNT":0.5405,"AMOUNT_CNT":10000,"SORT":8000,"LANG":{"ru":{"DECIMALS":2,"DEC_POINT":".","FORMAT_STRING":"Rp#","FULL_NAME":"rupiah","HIDE_ZERO":"Y","THOUSANDS_VARIANT":"C"},"en":{"DECIMALS":2,"DEC_POINT":".","FORMAT_STRING":"Rp#","FULL_NAME":"rupee","HIDE_ZERO":"Y","THOUSANDS_VARIANT":"C"}}},"auth":"**put_access_token_here**"}' \
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
                AMOUNT: 0.5405,
                AMOUNT_CNT: 10000,
                SORT: 8000,
                LANG: {
                  ru: {
                    DECIMALS: 2,
                    DEC_POINT: '.',
                    FORMAT_STRING: 'Rp#',
                    FULL_NAME: 'rupiah',
                    HIDE_ZERO: 'Y',
                    THOUSANDS_VARIANT: 'C',
                  },
                  en: {
                    DECIMALS: 2,
                    DEC_POINT: '.',
                    FORMAT_STRING: 'Rp#',
                    FULL_NAME: 'rupee',
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
                    AMOUNT: 0.5405,
                    AMOUNT_CNT: 10000,
                    SORT: 8000,
                    LANG: {
                      ru: {
                        DECIMALS: 2,
                        DEC_POINT: '.',
                        FORMAT_STRING: 'Rp#',
                        FULL_NAME: 'rupiah',
                        HIDE_ZERO: 'Y',
                        THOUSANDS_VARIANT: 'C',
                      },
                      en: {
                        DECIMALS: 2,
                        DEC_POINT: '.',
                        FORMAT_STRING: 'Rp#',
                        FULL_NAME: 'rupee',
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

    - Python

        ```python

        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        try:
            bitrix_response = client.crm.currency.add(
                fields={
                    "CURRENCY": "IDR",
                    "AMOUNT": 0.5405,
                    "AMOUNT_CNT": 10000,
                    "SORT": 8000,
                    "LANG": {
                        "de": {
                            "DECIMALS": 2,
                            "DEC_POINT": ".",
                            "FORMAT_STRING": "Rp#",
                            "FULL_NAME": "rupee",
                            "HIDE_ZERO": "Y",
                            "THOUSANDS_VARIANT": "C",
                        },
                        "en": {
                            "DECIMALS": 2,
                            "DEC_POINT": ".",
                            "FORMAT_STRING": "Rp#",
                            "FULL_NAME": "rupee",
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

    - PHP

        ```php
        try {
            $response = $b24Service
                ->core
                ->call(
                    'crm.currency.add',
                    [
                        'fields' => [
                            'CURRENCY' => 'IDR',
                            'AMOUNT' => 0.5405,
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
                                    'FORMAT_STRING' => 'Rp#',
                                    'FULL_NAME' => 'rupee',
                                    'HIDE_ZERO' => 'Y',
                                    'THOUSANDS_VARIANT' => 'C',
                                ],
                            ],
                        ],
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
            "crm.currency.add",
            {
                fields: {
                    CURRENCY: 'IDR',
                    AMOUNT: 0.5405,
                    AMOUNT_CNT: 10000,
                    SORT: 8000,
                    LANG: {
                        ru: {
                            DECIMALS: 2,
                            DEC_POINT: '.',
                            FORMAT_STRING: 'Rp#',
                            FULL_NAME: 'rupiah',
                            HIDE_ZERO: 'Y',
                            THOUSANDS_VARIANT: 'C'
                        },
                        en: {
                            DECIMALS: 2,
                            DEC_POINT: '.',
                            FORMAT_STRING: 'Rp#',
                            FULL_NAME: 'rupee',
                            HIDE_ZERO: 'Y',
                            THOUSANDS_VARIANT: 'C'
                        }
                    }
                }
            },
            function(result)
            {
                if (result.error())
                {
                    console.error(result.error());
                }
                else
                {
                    console.log(result.data());
                }
            }
        );
        ```

    - PHP CRest

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.currency.add',
            [
                'fields' => [
                    'CURRENCY' => 'IDR',
                    'AMOUNT' => 0.5405,
                    'AMOUNT_CNT' => 10000,
                    'SORT' => 8000,
                    'LANG' => [
                        'de' => [
                            'DECIMALS' => 2,
                            'DEC_POINT' => '.',
                            'FORMAT_STRING' => 'Rp#',
                            'FULL_NAME' => 'rupee',
                            'HIDE_ZERO' => 'Y',
                            'THOUSANDS_VARIANT' => 'C',
                        ],
                        'en' => [
                            'DECIMALS' => 2,
                            'DEC_POINT' => '.',
                            'FORMAT_STRING' => 'Rp#',
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

    - Go

        ```go
        // client and ctx are already created — see the Go SDK section
        res, err := client.Core().Call(ctx, "crm.currency.add", b24.Params{
        	"fields": b24.Params{
        		"CURRENCY":   "IDR",
        		"AMOUNT":     0.5405,
        		"AMOUNT_CNT": 10000,
        		"SORT":       8000,
        		"LANG": b24.Params{
        			"ru": b24.Params{
        				"DECIMALS":          2,
        				"DEC_POINT":         ".",
        				"FORMAT_STRING":     "Rp#",
        				"FULL_NAME":         "rupiah",
        				"HIDE_ZERO":         "Y",
        				"THOUSANDS_VARIANT": "C",
        			},
        			"en": b24.Params{
        				"DECIMALS":          2,
        				"DEC_POINT":         ".",
        				"FORMAT_STRING":     "Rp#",
        				"FULL_NAME":         "rupee",
        				"HIDE_ZERO":         "Y",
        				"THOUSANDS_VARIANT": "C",
        			},
        		},
        	},
        })
        if err != nil {
        	return fmt.Errorf("crm.currency.add: %w", err)
        }

        var value string
        if err := json.Unmarshal(res.Result, &value); err != nil {
        	return fmt.Errorf("parse response: %w", err)
        }
        fmt.Println("result:", value)
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
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | `Access denied.` | The user does not have the "Edit CRM preferences" permission in CRM ||
|| `400` | `ERROR_CORE` | `The currency ID must be represented by three Latin characters.` | `CURRENCY` is not three Latin letters ||
|| `400` | `ERROR_CORE` | `Currency with this ID already exists.` | A currency with this code already exists ||
|| `400` | `ERROR_CORE` | `The default exchange rate is not specified.` | `AMOUNT` is not passed ||
|| `400` | `ERROR_CORE` | `The field "Format" for language ru is empty.` | `FORMAT_STRING` is not passed in a localization from `LANG` ||
|| `400` | `ERROR_CORE` | `The decimal point is the same as the thousands separator specified for en` | In a localization from `LANG`, the thousands separator is the same as `DEC_POINT` ||
|| `400` | `ERROR_CORE` | `The default exchange rate is incorrect.` | `AMOUNT` is not greater than zero ||
|| `400` | `ERROR_CORE` | `The currency face value is incorrect.` | `AMOUNT_CNT` is not greater than zero ||
|| `400` | Empty value | `The Currency module is not installed.` | The Currency module is not installed ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-currency-update.md)
- [{#T}](./crm-currency-get.md)
- [{#T}](./crm-currency-list.md)
- [{#T}](./crm-currency-delete.md)
- [{#T}](./crm-currency-fields.md)
- [{#T}](./index.md)