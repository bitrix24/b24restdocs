# Create a New Bank Detail crm.requisite.bankdetail.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method creates a new bank detail.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | A set of fields — an object of the form `{"field": "value"[, ...]}` for adding a bank detail ||
|#

### Parameter fields

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_ID***
[`integer`](../../../data-types.md) | Parent object identifier. Currently, it can only be an attribute identifier. Attribute identifiers can be obtained using the [`crm.requisite.list`](../universal/crm-requisite-list.md) method. ||
|| **COUNTRY_ID**
[`integer`](../../../data-types.md) | Identifier of the country corresponding to the set of bank details fields (see method [crm.requisite.preset.countries](../presets/crm-requisite-preset-countries.md) for available values).

The country code of the bank details matches the country code in the linked requisite template, the identifier of which is specified in the `ENTITY_ID` field 
||
|| **NAME***
[`string`](../../../data-types.md) | Name of the bank details ||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the requisite ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key. Used for exchange operations. Identifier of the object in the external information base. 

The purpose of the field may change by the final developer. Each application ensures the uniqueness of values in this field. 

It is recommended to use a unique prefix to avoid collisions with other applications ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity indicator. Uses values `Y` or `N`. 

Currently, the field does not actually affect anything ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting ||
|| **RQ_BANK_NAME**
[`string`](../../../data-types.md) | Name of the bank ||
|| **RQ_BANK_ADDR**
[`string`](../../../data-types.md) | Address of the bank ||
|| **RQ_BANK_CODE**
[`string`](../../../data-types.md) | Bank Code (for country BR) ||
|| **RQ_BANK_ROUTE_NUM**
[`string`](../../../data-types.md) | Bank Routing Number ||
|| **RQ_BIK**
[`string`](../../../data-types.md) | BIK ||
|| **RQ_CODEB**
[`string`](../../../data-types.md) | Bank Code (for country FR) ||
|| **RQ_CODEG**
[`string`](../../../data-types.md) | Branch Code (for country FR) ||
|| **RQ_RIB**
[`string`](../../../data-types.md) | RIB Key (for country FR) ||
|| **RQ_MFO**
[`string`](../../../data-types.md) | MFO ||
|| **RQ_ACC_NAME**
[`string`](../../../data-types.md) | Bank Account Holder Name ||
|| **RQ_ACC_NUM**
[`string`](../../../data-types.md) | Bank Account Number ||
|| **RQ_ACC_TYPE**
[`string`](../../../data-types.md) | Account Type (for country BR) ||
|| **RQ_AGENCY_NAME**
[`string`](../../../data-types.md) | Agency (for country BR) ||
|| **RQ_IIK**
[`string`](../../../data-types.md) | IIK ||
|| **RQ_ACC_CURRENCY**
[`string`](../../../data-types.md) | Currency of the account ||
|| **RQ_COR_ACC_NUM**
[`string`](../../../data-types.md) | Correspondent account ||
|| **RQ_IBAN**
[`string`](../../../data-types.md) | IBAN ||
|| **RQ_SWIFT**
[`string`](../../../data-types.md) | SWIFT ||
|| **RQ_BIK**
[`string`](../../../data-types.md) | BIK ||
|| **COMMENTS**
[`string`](../../../data-types.md) | Comment ||
|| **ORIGINATOR_ID**
[`string`](../../../data-types.md) | Identifier of the external information base. The purpose of the field may change by the final developer ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ENTITY_ID":27,"COUNTRY_ID":1,"NAME":"Superbank","RQ_BANK_NAME":"Ltd. Superbank","RQ_BANK_ADDR":"117312, New York, 19 Miller St.","RQ_BIK":"044525225","RQ_ACC_NUM":"40702810938000060473","RQ_ACC_CURRENCY":"USD","RQ_COR_ACC_NUM":"30101810400000000225","XML_ID":"1e4641fd-2dd9-31e6-b2f2-105056c00008","ACTIVE":"Y","SORT":600}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.bankdetail.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ENTITY_ID":27,"COUNTRY_ID":1,"NAME":"Superbank","RQ_BANK_NAME":"Ltd. Superbank","RQ_BANK_ADDR":"117312, New York, 19 Miller St.","RQ_BIK":"044525225","RQ_ACC_NUM":"40702810938000060473","RQ_ACC_CURRENCY":"USD","RQ_COR_ACC_NUM":"30101810400000000225","XML_ID":"1e4641fd-2dd9-31e6-b2f2-105056c00008","ACTIVE":"Y","SORT":600},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.bankdetail.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'crm.requisite.bankdetail.add',
        params: {
          fields: {
            ENTITY_ID: 27,           // Requisite ID
            COUNTRY_ID: 1,           // Country code
            NAME: 'Superbank',       // Bank detail name
            RQ_BANK_NAME: 'JSC Superbank',  // Bank name
            RQ_BANK_ADDR: '117312, New York, 19 Miller St.',
            RQ_BIK: '044525225',
            RQ_ACC_NUM: '40702810938000060473',
            RQ_ACC_CURRENCY: 'USD',
            RQ_COR_ACC_NUM: '30101810400000000225',
            XML_ID: '1e4641fd-2dd9-31e6-b2f2-105056c00008',
            ACTIVE: 'Y',
            SORT: 600,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created bank detail with ID:', result)
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
      async function addBankDetail() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.requisite.bankdetail.add',
            params: {
              fields: {
                ENTITY_ID: 27,           // Requisite ID
                COUNTRY_ID: 1,           // Country code (Russia)
                NAME: 'Superbank',       // Bank detail name
                RQ_BANK_NAME: 'JSC Superbank',  // Bank name
                RQ_BANK_ADDR: '117312, New York, 19 Miller St.',
                RQ_BIK: '044525225',
                RQ_ACC_NUM: '40702810938000060473',
                RQ_ACC_CURRENCY: 'USD',
                RQ_COR_ACC_NUM: '30101810400000000225',
                XML_ID: '1e4641fd-2dd9-31e6-b2f2-105056c00008',
                ACTIVE: 'Y',
                SORT: 600,
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
          console.info('Created bank detail with ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addBankDetail)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.requisite.bankdetail.add',
                [
                    'fields' => [
                        'ENTITY_ID'       => 27,
                        'COUNTRY_ID'      => 1,
                        'NAME'            => 'Superbank',
                        'RQ_BANK_NAME'    => 'Ltd. Superbank',
                        'RQ_BANK_ADDR'    => '117312, New York, 19 Miller St.',
                        'RQ_BIK'          => '044525225',
                        'RQ_ACC_NUM'      => '40702810938000060473',
                        'RQ_ACC_CURRENCY' => 'USD',
                        'RQ_COR_ACC_NUM'  => '30101810400000000225',
                        'XML_ID'          => '1e4641fd-2dd9-31e6-b2f2-105056c00008',
                        'ACTIVE'          => 'Y',
                        'SORT'            => 600,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Bank details created with ID ' . $result;
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating bank detail: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.bankdetail.add",
        {
            fields:
            {
                "ENTITY_ID": 27,                 // Identifier of the detail
                "COUNTRY_ID": 1,                 // Country code (USA)
                "NAME": "Superbank",              // Name of the bank detail
                "RQ_BANK_NAME": "Ltd. Superbank",  // Bank name
                "RQ_BANK_ADDR": "117312, New York, 19 Miller St.",
                "RQ_BIK": "044525225",
                "RQ_ACC_NUM": "40702810938000060473",
                "RQ_ACC_CURRENCY": "USD",
                "RQ_COR_ACC_NUM": "30101810400000000225",
                "XML_ID":"1e4641fd-2dd9-31e6-b2f2-105056c00008",
                "ACTIVE":"Y",
                "SORT":600
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Bank details created with ID " + result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.bankdetail.add',
        [
            'fields' => [
                'ENTITY_ID' => 27,
                'COUNTRY_ID' => 1,
                'NAME' => 'Superbank',
                'RQ_BANK_NAME' => 'Ltd. Superbank',
                'RQ_BANK_ADDR' => '117312, New York, 19 Miller St.',
                'RQ_BIK' => '044525225',
                'RQ_ACC_NUM' => '40702810938000060473',
                'RQ_ACC_CURRENCY' => 'USD',
                'RQ_COR_ACC_NUM' => '30101810400000000225',
                'XML_ID' => '1e4641fd-2dd9-31e6-b2f2-105056c00008',
                'ACTIVE' => 'Y',
                'SORT' => 600
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
        bitrix_response = client.crm.requisite.bankdetail.add(
            fields={
                "ENTITY_ID": 27,
                "COUNTRY_ID": 1,
                "NAME": "Superbank",
                "RQ_BANK_NAME": "Ltd. Superbank",
                "RQ_BANK_ADDR": "117312, New York, 19 Miller St.",
                "RQ_BIK": "044525225",
                "RQ_ACC_NUM": "40702810938000060473",
                "RQ_ACC_CURRENCY": "USD",
                "RQ_COR_ACC_NUM": "30101810400000000225",
                "XML_ID": "1e4641fd-2dd9-31e6-b2f2-105056c00008",
                "ACTIVE": "Y",
                "SORT": 600,
            },
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

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 357,
    "time": {
        "start": 1717429942.060649,
        "finish": 1717429942.626925,
        "duration": 0.5662760734558105,
        "processing": 0.09111285209655762,
        "date_start": "2024-06-03T17:52:22+02:00",
        "date_finish": "2024-06-03T17:52:22+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the created bank detail ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "ENTITY_ID is not defined or invalid."
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|
|| **Error text** | **Description** ||
|| `ENTITY_ID is not defined or invalid` | The identifier of the detail is not defined or has an invalid value ||
|| `Access denied` | Insufficient access permissions to add a bank detail ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-bank-detail-update.md)
- [{#T}](./crm-requisite-bank-detail-get.md)
- [{#T}](./crm-requisite-bank-detail-list.md)
- [{#T}](./crm-requisite-bank-detail-delete.md)
- [{#T}](./crm-requisite-bank-detail-fields.md)
