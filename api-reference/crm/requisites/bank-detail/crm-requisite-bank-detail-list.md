# Get a List of Bank Details by Filter crm.requisite.bankdetail.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

Returns a list of banking details based on the filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | The array contains a list of fields to select (see [bank detail fields](#fields)).

If the array is not provided or an empty array is passed, all available bank detail fields will be selected. ||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the selected bank details in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to [bank detail fields](#fields).

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol does not need to be passed in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol does not need to be passed in the filter value. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal 
||
|| **order**
[`object`](../../../data-types.md) | An object for sorting the selected bank details in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to [bank detail fields](#fields).

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../../data-types.md) | This parameter is used to manage pagination.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number 
||
|#

### Description of Bank Detail Fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the bank details. Automatically created and unique within the account ||
|| **ENTITY_ID**
[`integer`](../../../data-types.md) | Identifier of the parent object. Currently can only be the identifier of the requisite. 

Identifiers of requisites can be obtained using the method [`crm.requisite.list`](../universal/crm-requisite-list.md) ||
|| **COUNTRY_ID**
[`integer`](../../../data-types.md) | Identifier of the country corresponding to the set of bank details fields (see method [crm.requisite.preset.countries](../presets/crm-requisite-preset-countries.md) for available values).

The country code of the bank details matches the country code in the linked requisite template, the identifier of which is specified in the `ENTITY_ID` field 
||
|| **DATE_CREATE**
[`datetime`](../../../data-types.md) | Create date ||
|| **DATE_MODIFY**
[`datetime`](../../../data-types.md) | Modification date ||
|| **CREATED_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who created the requisite ||
|| **MODIFY_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who modified the requisite ||
|| **NAME**
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
    -d '{"order":{"DATE_CREATE":"ASC"},"filter":{"COUNTRY_ID":"1"},"select":["ENTITY_ID","ID","NAME"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.bankdetail.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"DATE_CREATE":"ASC"},"filter":{"COUNTRY_ID":"1"},"select":["ENTITY_ID","ID","NAME"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.bankdetail.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each bank detail item returned in result[]
    type BankDetailItem = {
      ID: string
      ENTITY_ID: string
      NAME: string
    }

    try {
      // crm.requisite.bankdetail.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<BankDetailItem[]>({
        method: 'crm.requisite.bankdetail.list',
        params: {
          order: { DATE_CREATE: 'ASC' },
          filter: { COUNTRY_ID: '1' },
          select: ['ENTITY_ID', 'ID', 'NAME'],
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Bank details count:', result.length, 'Items:', result)
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
      async function listBankDetails() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.requisite.bankdetail.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.requisite.bankdetail.list',
            params: {
              order: { DATE_CREATE: 'ASC' },
              filter: { COUNTRY_ID: '1' },
              select: ['ENTITY_ID', 'ID', 'NAME'],
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
          console.info('Bank details count:', result.length, 'Items:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listBankDetails)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.requisite.bankdetail.list',
                [
                    'order'  => ['DATE_CREATE' => 'ASC'],
                    'filter' => ['COUNTRY_ID' => '1'],
                    'select' => ['ENTITY_ID', 'ID', 'NAME'],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
        if ($result->more()) {
            $result->next();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing bank details: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.bankdetail.list",
        {
            order: { "DATE_CREATE": "ASC" },
            filter: { "COUNTRY_ID": "1" },
            select: [ "ENTITY_ID", "ID", "NAME" ]
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.dir(result.data());
                if(result.more())
                    result.next();
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.bankdetail.list',
        [
            'order' => ['DATE_CREATE' => 'ASC'],
            'filter' => ['COUNTRY_ID' => '1'],
            'select' => ['ENTITY_ID', 'ID', 'NAME']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.requisite.bankdetail.list(
            order={"DATE_CREATE": "ASC"},
            filter={"COUNTRY_ID": "1"},
            select=["ENTITY_ID", "ID", "NAME"],
            start=0,
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
        bitrix_response = client.crm.requisite.bankdetail.list(
            order={"DATE_CREATE": "ASC"},
            filter={"COUNTRY_ID": "1"},
            select=["ENTITY_ID", "ID", "NAME"],
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
        bitrix_response = client.crm.requisite.bankdetail.list(
            filter={"COUNTRY_ID": "1"},
            select=["ENTITY_ID", "ID", "NAME"],
            order={"ID": "DESC"},
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

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "ENTITY_ID": "27",
            "ID": "11",
            "NAME": "Chase"
        },
        {
            "ENTITY_ID": "30",
            "ID": "15",
            "NAME": "Alpha Bank Corp."
        },
        {
            "ENTITY_ID": "30",
            "ID": "16",
            "NAME": "Chase Bank Corp."
        },
        {
            "ENTITY_ID": "45",
            "ID": "17",
            "NAME": "Chase Bank Corp."
        },
        {
            "ENTITY_ID": "45",
            "ID": "18",
            "NAME": "Alpha Bank Corp."
        }
    ],
    "total": 5,
    "time": {
        "start": 1717498684.70562,
        "finish": 1717498685.13295,
        "duration": 0.42733001708984375,
        "processing": 0.020370006561279297,
        "date_start": "2024-06-04T12:58:04+02:00",
        "date_finish": "2024-06-04T12:58:05+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md)| An array of objects with information from the selected bank details. Each element contains the selected [bank detail fields](#fields). ||
|| **total**
[`integer`](../../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|
|| **Error text** | **Description** ||
|| `Access denied` | Insufficient access permissions to retrieve the list of bank details. ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-bank-detail-add.md)
- [{#T}](./crm-requisite-bank-detail-update.md)
- [{#T}](./crm-requisite-bank-detail-get.md)
- [{#T}](./crm-requisite-bank-detail-delete.md)
- [{#T}](./crm-requisite-bank-detail-fields.md)