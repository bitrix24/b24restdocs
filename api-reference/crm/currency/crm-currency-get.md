# Get Currency by Code crm.currency.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with read access to at least one CRM object

The `crm.currency.get` method returns a currency by its code, for example, `USD`. The response contains the exchange rate to the base currency, the sort index, and display parameters for different languages.

{% note info %}

The method returns the currency name, amount format, and separators for the current Bitrix24 language. Localizations for all languages are returned in the `LANG` field.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type`| **Description** ||
|| **id***
[`crm_currency.CURRENCY`](../data-types.md#crm_currency) | Symbolic identifier of the currency.

Corresponds to the ISO 4217 standard.

The value is case-insensitive: the method accepts `usd` as `USD`.

The code can be obtained using the [crm.currency.list](./crm-currency-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"USD"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.currency.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"USD","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.currency.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CrmCurrencyResult = {
      CURRENCY: string
      AMOUNT_CNT: string
      AMOUNT: string
      SORT: string
      BASE: string
      FULL_NAME: string
      LID: string
      FORMAT_STRING: string
      DEC_POINT: string
      THOUSANDS_SEP: string | null
      DECIMALS: string
      DATE_UPDATE: ISODate | null
      LANG: Record<string, CrmCurrencyLang>
    }

    type CrmCurrencyLang = {
      FORMAT_STRING: string
      FULL_NAME: string
      DEC_POINT: string
      THOUSANDS_SEP: string | null
      DECIMALS: string
      THOUSANDS_VARIANT: string
      HIDE_ZERO: string
    }

    try {
      const response = await $b24.actions.v2.call.make<CrmCurrencyResult>({
        method: 'crm.currency.get',
        params: {
          id: 'USD',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Currency:', result.CURRENCY, result.FULL_NAME)
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
      async function getCurrency() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.currency.get',
            params: {
              id: 'USD',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Currency:', result.CURRENCY, result.FULL_NAME)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCurrency)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.currency.get(bitrix_id="USD").response
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
                'crm.currency.get',
                [
                    'id' => 'USD',
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
        "crm.currency.get",
        {
            id: "USD"
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
        'crm.currency.get',
        [
            'id' => 'USD'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.currency.get", b24.Params{
    	"id": "EUR",
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.currency.get: %w", err)
    }

    var item struct {
    	Currency  string `json:"CURRENCY"`
    	AmountCnt string `json:"AMOUNT_CNT"`
    	Amount    string `json:"AMOUNT"`
    	Sort      string `json:"SORT"`
    	Base      string `json:"BASE"`
    	FullName  string `json:"FULL_NAME"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Currency, item.AmountCnt)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "CURRENCY": "USD",
        "AMOUNT_CNT": "1",
        "AMOUNT": "1.0000",
        "SORT": "100",
        "BASE": "Y",
        "FULL_NAME": "United States Dollar",
        "LID": "de",
        "FORMAT_STRING": "# $",
        "DEC_POINT": ".",
        "THOUSANDS_SEP": "&nbsp;",
        "DECIMALS": "2",
        "DATE_UPDATE": "2024-01-29T12:28:40+02:00",
        "LANG": {
            "en": {
                "FORMAT_STRING": "$#",
                "FULL_NAME": "United States Dollar",
                "DEC_POINT": ".",
                "THOUSANDS_SEP": null,
                "DECIMALS": "2",
                "THOUSANDS_VARIANT": "C",
                "HIDE_ZERO": "Y"
            },
            "de": {
                "FORMAT_STRING": "# $",
                "FULL_NAME": "United States Dollar",
                "DEC_POINT": ".",
                "THOUSANDS_SEP": "&nbsp;",
                "DECIMALS": "2",
                "THOUSANDS_VARIANT": "B",
                "HIDE_ZERO": "Y"
            }
        }
    },
    "time": {
        "start": 1718357792.091095,
        "finish": 1718357792.889212,
        "duration": 0.79811692237854,
        "processing": 0.10800814628601074,
        "date_start": "2024-06-14T11:36:32+02:00",
        "date_finish": "2024-06-14T11:36:32+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`crm_currency`](../data-types.md#crm_currency) | Currency data [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

#### result Object {#result}

Numeric values are returned as strings. The fields `FULL_NAME`, `LID`, `FORMAT_STRING`, `DEC_POINT`, `THOUSANDS_SEP`, and `DECIMALS` refer to the current Bitrix24 language. If there is no localization for this language, `FULL_NAME` equals the currency code, and the other fields are `null`.

#|
|| **Name**
`type` | **Description** ||
|| **CURRENCY**
[`string`](../../data-types.md) | Currency code according to ISO 4217, for example, `USD` ||
|| **AMOUNT_CNT**
[`string`](../../data-types.md) | Nominal value — the number of currency units for which the rate is specified, for example, `1` ||
|| **AMOUNT**
[`string`](../../data-types.md) | Exchange rate of the nominal value to the base currency, for example, `0.9250` ||
|| **SORT**
[`string`](../../data-types.md) | Sort index ||
|| **BASE**
[`string`](../../data-types.md) | Whether the currency is the base currency: `Y` or `N` ||
|| **FULL_NAME**
[`string`](../../data-types.md) | Currency name in the current language ||
|| **LID**
[`string`](../../data-types.md) | Language for which the display fields are returned ||
|| **FORMAT_STRING**
[`string`](../../data-types.md) | Amount display template, for example, `# $` ||
|| **DEC_POINT**
[`string`](../../data-types.md) | Decimal separator ||
|| **THOUSANDS_SEP**
[`string`](../../data-types.md) | Thousands separator ||
|| **DECIMALS**
[`string`](../../data-types.md) | Number of decimal places ||
|| **DATE_UPDATE**
[`string`](../../data-types.md) | Date and time of the last change in ISO 8601 format ||
|| **LANG**
[`object`](../../data-types.md) | Currency localizations by language in the format `{"en": {...}, "de": {...}}`. The localization fields are described in the [crm_currency_localization](../data-types.md#crm_currency_localization) type. If there are no localizations, an empty array `[]` ||
|#

## Error Handling

HTTP status: **400**

```json
{
	"error": "",
	"error_description": "Not found"
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | `Access denied.` | The user has no read access to any CRM object ||
|| `400` | Empty value | `ID is not defined or invalid.` | The `id` parameter is not passed ||
|| `400` | Empty value | `The parameter id is not string.` | `id` is not passed as a string ||
|| `400` | Empty value | `Not found` | No currency with this code exists ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-currency-add.md)
- [{#T}](./crm-currency-update.md)
- [{#T}](./crm-currency-list.md)
- [{#T}](./crm-currency-delete.md)
- [{#T}](./crm-currency-fields.md)
- [{#T}](./index.md)