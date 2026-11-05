# Update Currency crm.currency.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Edit CRM preferences" permission in CRM

The `crm.currency.update` method changes the exchange rate, sort order, and localizations of a currency.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type`| **Description** ||
|| **ID***
[`string`](../../data-types.md) | Currency identifier.

Corresponds to the ISO 4217 standard.

The identifier can be obtained using the [crm.currency.list](./crm-currency-list.md) method.
 ||
|| **fields**
[`object`](../../data-types.md) | Field values (detailed description provided [below](#parameter-fields)) for updating the currency in the following structure:

```js
fields: {
    SORT: 'value',
    AMOUNT_CNT: 'value',
    AMOUNT: 'value',
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

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type`| **Description** ||
|| **SORT**
[`integer`](../../data-types.md) | Position in the currency list.

If not passed, the value does not change
 ||
|| **AMOUNT_CNT**
[`integer`](../../data-types.md) | Denomination — the number of currency units for which the `AMOUNT` rate is set. Usually `1`, `10`, `100`, and so on
 ||
|| **AMOUNT**
[`double`](../../data-types.md) | The value of the `AMOUNT_CNT` denomination in the base currency, for example `15.3449` ||
|| **BASE**
[`string`](../../data-types.md) | Base currency flag. The method ignores this parameter and does not return an error: you cannot make a currency the base currency with this method. The base currency is changed by the [crm.currency.base.set](./crm-currency-base-set.md) method
 ||
|| **LANG**
[`object`](../../data-types.md) | Currency localization parameters.

An object in the format `{"lang_1": "value_1", ... "lang_N": "value_N"}`, where `lang_N` is the language identifier, and `value` is an object of type [crm_currency_localization](../data-types.md#crm_currency_localization).

The method changes localizations only for the languages passed in `LANG`. The others stay unchanged
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
|| **FORMAT_STRING**
[`string`](../../data-types.md) | Amount display template, for example `$#`. Required if the language does not have a localization yet ||
|| **FULL_NAME**
[`string`](../../data-types.md) | Name of the currency in the language for which localization is added. ||
|| **HIDE_ZERO**
[`string`](../../data-types.md) | Whether to hide insignificant zeros: `Y` — hide, `N` — show ||
|| **THOUSANDS_SEP**
[`string`](../../data-types.md) | Thousands separator ||
|| **THOUSANDS_VARIANT**
[`string`](../../data-types.md) | Code for the thousands separator.

Refer to the [crm_currency_localization](../data-types.md#crm_currency_localization) documentation for acceptable values. ||
|#

## Code Examples

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

    - Python

        ```python

        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

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

    - PHP

        ```php
        try {
            $response = $b24Service
                ->core
                ->call(
                    'crm.currency.update',
                    [
                        'ID' => 'CNY',
                        'fields' => [
                            'AMOUNT' => 15.3449,
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
            "crm.currency.update",
            {
                ID: 'CNY',
                fields: {
                    AMOUNT: 15.3449
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

    - Go

        ```go
        // client and ctx are already created — see the Go SDK section
        res, err := client.Core().Call(ctx, "crm.currency.update", b24.Params{
        	"ID": "CNY",
        	"fields": b24.Params{
        		"AMOUNT": 15.3449,
        	},
        })
        if err != nil {
        	return fmt.Errorf("crm.currency.update: %w", err)
        }

        var ok bool
        if err := json.Unmarshal(res.Result, &ok); err != nil {
        	return fmt.Errorf("parse response: %w", err)
        }
        fmt.Println("done:", ok)
        ```

    {% endlist %}

2. Changing the localizations of the currency (using the example of the US dollar)

    The example changes or adds the localizations of the US dollar `USD` only for English and German. Localizations in other languages, if any, do not change.

    The [crm.currency.localizations.set](./localizations/crm-currency-localizations-set.md) method works in a similar way.

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

    - Python

        ```python

        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

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

    - PHP

        ```php
        try {
            $response = $b24Service
                ->core
                ->call(
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
            "crm.currency.update",
            {
                ID: 'USD',
                fields: {
                    LANG: {
                        en: {
                            DECIMALS: 2,
                            DEC_POINT: '.',
                            FORMAT_STRING: '$#',
                            FULL_NAME: 'US Dollar',
                            HIDE_ZERO: 'Y',
                            THOUSANDS_VARIANT: 'S'
                        },
                        de: {
                            DECIMALS: 2,
                            DEC_POINT: '.',
                            FORMAT_STRING: '# $',
                            FULL_NAME: 'US-Dollar',
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

    - Go

        ```go
        // client and ctx are already created — see the Go SDK section
        res, err := client.Core().Call(ctx, "crm.currency.update", b24.Params{
        	"ID": "USD",
        	"fields": b24.Params{
        		"LANG": b24.Params{
        			"en": b24.Params{
        				"DECIMALS":          2,
        				"DEC_POINT":         ".",
        				"FORMAT_STRING":     "$#",
        				"FULL_NAME":         "US Dollar",
        				"HIDE_ZERO":         "Y",
        				"THOUSANDS_VARIANT": "S",
        			},
        			"de": b24.Params{
        				"DECIMALS":          2,
        				"DEC_POINT":         ".",
        				"FORMAT_STRING":     "# $",
        				"FULL_NAME":         "US-Dollar",
        				"HIDE_ZERO":         "Y",
        				"THOUSANDS_VARIANT": "C",
        			},
        		},
        	},
        })
        if err != nil {
        	return fmt.Errorf("crm.currency.update: %w", err)
        }

        var ok bool
        if err := json.Unmarshal(res.Result, &ok); err != nil {
        	return fmt.Errorf("parse response: %w", err)
        }
        fmt.Println("done:", ok)
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
[`boolean`](../../data-types.md) | `true` if the currency is updated ||
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
|| `400` | Empty value | `ID is not defined or invalid.` | `ID` is not passed ||
|| `400` | Empty value | `The parameter id is not string.` | `ID` is not passed as a string ||
|| `400` | Empty value | `Parameter 'fields' must be array.` | `fields` is not passed as an object ||
|| `400` | Empty value | `Access denied.` | The user does not have the "Edit CRM preferences" permission in CRM ||
|| `400` | Empty value | `Currency is not found` | There is no currency with this code ||
|| `400` | `ERROR_CORE` | `The default exchange rate is incorrect.` | `AMOUNT` is not greater than zero ||
|| `400` | `ERROR_CORE` | `The currency face value is incorrect.` | `AMOUNT_CNT` is not greater than zero ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-currency-add.md)
- [{#T}](./crm-currency-get.md)
- [{#T}](./crm-currency-list.md)
- [{#T}](./crm-currency-delete.md)
- [{#T}](./crm-currency-fields.md)
- [{#T}](./index.md)