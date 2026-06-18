# Update Bank Details crm.requisite.bankdetail.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates an existing bank detail.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Parameter** | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the bank detail. 

Bank detail identifiers can be obtained using the [`crm.requisite.bankdetail.list`](./crm-requisite-bank-detail-list.md) method. ||
|| **fields***
[`object`](../../../data-types.md) | Set of bank detail fields — an object of the form `{"field": "value"[, ...]}`, the values of which need to be changed. ||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the bank detail. ||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the detail. ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key. Used for exchange operations. Identifier of the external information base object. 

The purpose of the field may change by the final developer. Each application ensures the uniqueness of values in this field. 

It is recommended to use a unique prefix to avoid collisions with other applications. ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity status. Values `Y` or `N` are used. 

Currently, this field does not affect anything. ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting. ||
|| **RQ_BANK_NAME**
[`string`](../../../data-types.md) | Name of the bank. ||
|| **RQ_BANK_ADDR**
[`string`](../../../data-types.md) | Address of the bank. ||
|| **RQ_BANK_CODE**
[`string`](../../../data-types.md) | Bank Code (for country BR). ||
|| **RQ_BANK_ROUTE_NUM**
[`string`](../../../data-types.md) | Bank Routing Number. ||
|| **RQ_BIK**
[`string`](../../../data-types.md) | BIK. ||
|| **RQ_CODEB**
[`string`](../../../data-types.md) | Code Banque (for country FR). ||
|| **RQ_CODEG**
[`string`](../../../data-types.md) | Code Guichet (for country FR). ||
|| **RQ_RIB**
[`string`](../../../data-types.md) | Clé RIB (for country FR). ||
|| **RQ_MFO**
[`string`](../../../data-types.md) | MFO. ||
|| **RQ_ACC_NAME**
[`string`](../../../data-types.md) | Bank Account Holder Name. ||
|| **RQ_ACC_NUM**
[`string`](../../../data-types.md) | Bank Account Number. ||
|| **RQ_ACC_TYPE**
[`string`](../../../data-types.md) | Tipo da conta (for country BR). ||
|| **RQ_AGENCY_NAME**
[`string`](../../../data-types.md) | Agência (for country BR). ||
|| **RQ_IIK**
[`string`](../../../data-types.md) | IIK. ||
|| **RQ_ACC_CURRENCY**
[`string`](../../../data-types.md) | Account Currency. ||
|| **RQ_COR_ACC_NUM**
[`string`](../../../data-types.md) | Correspondent Account Number. ||
|| **RQ_IBAN**
[`string`](../../../data-types.md) | IBAN. ||
|| **RQ_SWIFT**
[`string`](../../../data-types.md) | SWIFT. ||
|| **RQ_BIC**
[`string`](../../../data-types.md) | BIC. ||
|| **COMMENTS**
[`string`](../../../data-types.md) | Comment. ||
|| **ORIGINATOR_ID**
[`string`](../../../data-types.md) | Identifier of the external information base. The purpose of the field may change by the final developer. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":357,"fields":{"NAME":"Superbank Ltd. (do not use)","COMMENTS":"Outdated","SORT":10000,"ACTIVE":"N"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.bankdetail.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":357,"fields":{"NAME":"Superbank Ltd. (do not use)","COMMENTS":"Outdated","SORT":10000,"ACTIVE":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.bankdetail.update
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
        method: 'crm.requisite.bankdetail.update',
        params: {
          id: 357,
          fields: {
            NAME: 'PAO Superbank (do not use)',
            COMMENTS: 'Deprecated',
            SORT: 10000,
            ACTIVE: 'N',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Bank detail updated:', result)
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
      async function updateBankDetail() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.requisite.bankdetail.update',
            params: {
              id: 357,
              fields: {
                NAME: 'PAO Superbank (do not use)',
                COMMENTS: 'Deprecated',
                SORT: 10000,
                ACTIVE: 'N',
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
          console.info('Bank detail updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateBankDetail)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.requisite.bankdetail.update',
                [
                    'id'     => 357,
                    'fields' => [
                        'NAME'     => 'Superbank Ltd. (do not use)',
                        'COMMENTS' => 'Outdated',
                        'SORT'     => 10000,
                        'ACTIVE'   => 'N',
                    ],
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
        echo 'Error updating bank detail: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.bankdetail.update",
        {
            id: 357,
            fields:
            {
                "NAME": "Superbank Ltd. (do not use)",
                "COMMENTS": "Outdated",
                "SORT" : 10000,
                "ACTIVE": "N"
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.bankdetail.update',
        [
            'id' => 357,
            'fields' => [
                'NAME' => 'Superbank Ltd. (do not use)',
                'COMMENTS' => 'Outdated',
                'SORT' => 10000,
                'ACTIVE' => 'N'
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
        "start": 1717509116.239588,
        "finish": 1717509116.78087,
        "duration": 0.5412819385528564,
        "processing": 0.173170804977417,
        "date_start": "2024-06-04T15:51:56+02:00",
        "date_finish": "2024-06-04T15:51:56+02:00",
        "operating": 0.17314410209655762
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of updating the bank detail:
- true — updated
- false — not updated 
||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time. ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "The RequisiteBankDetail with ID '357' is not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Error Text** | **Description** ||
|| `ID is not defined or invalid` | The identifier of the bank detail is not specified or has an invalid value. ||
|| `The Requisite with ID '357' is not found` | The bank detail with the specified identifier was not found. ||
|| `Access denied` | Insufficient access permissions to update the bank detail. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-bank-detail-add.md)
- [{#T}](./crm-requisite-bank-detail-get.md)
- [{#T}](./crm-requisite-bank-detail-list.md)
- [{#T}](./crm-requisite-bank-detail-delete.md)
- [{#T}](./crm-requisite-bank-detail-fields.md)