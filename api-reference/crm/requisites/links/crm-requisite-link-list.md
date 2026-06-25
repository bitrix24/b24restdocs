# Get a List of Links for Requisites crm.requisite.link.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

Retrieves a list of links for company details based on the filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | An array of fields to select (see [requisite link fields](#fields)).

If the array is not provided or an empty array is passed, all available fields for requisite links will be selected. ||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the selected requisite links in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to [requisite link fields](#fields).

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal 
||
|| **order**
[`object`](../../../data-types.md) | An object for sorting the selected requisite links in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to [requisite link fields](#fields).

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

### Description of the Requisite Link Fields with the CRM Object {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the object type to which the link belongs.

The following types can be used:
- deal (value `2`)
- old invoice (value `5`)
- estimate (value `7`)
- new invoice (value `31`)
- other dynamic objects (to get possible values, see the method [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md)).

Object type identifiers can be obtained using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) 
||
|| **ENTITY_ID**
[`integer`](../../../data-types.md) | Identifier of the object to which the link belongs. 

Object identifiers can be obtained using the following methods: [crm.deal.list](../../deals/crm-deal-list.md), [crm.quote.list](../../quote/crm-quote-list.md), [crm.item.list](../../universal/crm-item-list.md) ||
|| **REQUISITE_ID**
[`integer`](../../../data-types.md) | Identifier of the client's requisite selected for the object. 

Requisite identifiers can be obtained using the method [crm.requisite.list](../universal/crm-requisite-list.md) ||
|| **BANK_DETAIL_ID**
[`integer`](../../../data-types.md) | Identifier of the client's bank requisite selected for the object.

Bank requisite identifiers can be obtained using the method [crm.requisite.bankdetail.list](../bank-detail/crm-requisite-bank-detail-list.md) ||
|| **MC_REQUISITE_ID**
[`integer`](../../../data-types.md) | Identifier of my company's requisite selected for the object. 

Requisite identifiers can be obtained using the method [crm.requisite.list](../universal/crm-requisite-list.md) ||
|| **MC_BANK_DETAIL_ID**
[`integer`](../../../data-types.md) | Identifier of my company's bank requisite selected for the object. 

Bank requisite identifiers can be obtained using the method [crm.requisite.bankdetail.list](../bank-detail/crm-requisite-bank-detail-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ENTITY_ID":"ASC"},"filter":{"@ENTITY_TYPE_ID":[1,2,7,31]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.link.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ENTITY_ID":"ASC"},"filter":{"@ENTITY_TYPE_ID":[1,2,7,31]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.link.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each item returned in result[]
    type RequisiteLinkItem = {
      ENTITY_TYPE_ID: string
      ENTITY_ID: string
      REQUISITE_ID: string
      BANK_DETAIL_ID: string
      MC_REQUISITE_ID: string
      MC_BANK_DETAIL_ID: string
    }

    try {
      // crm.requisite.link.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<RequisiteLinkItem[]>({
        method: 'crm.requisite.link.list',
        params: {
          order: { ENTITY_ID: 'ASC' },
          filter: { '@ENTITY_TYPE_ID': [1, 2, 7, 31] }, // Leads, deals, quotes, invoices
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Requisite links count:', result.length, 'First item:', result[0])
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
      async function listRequisiteLinks() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.requisite.link.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.requisite.link.list',
            params: {
              order: { ENTITY_ID: 'ASC' },
              filter: { '@ENTITY_TYPE_ID': [1, 2, 7, 31] }, // Leads, deals, quotes, invoices
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
          console.info('Requisite links count:', result.length, 'First item:', result[0])
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listRequisiteLinks)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.requisite.link.list',
                [
                    'order' => ['ENTITY_ID' => 'ASC'],
                    'filter' => ['@ENTITY_TYPE_ID' => [1, 2, 7, 31]],
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
        echo 'Error fetching requisite links: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.link.list", {
            order: {"ENTITY_ID": "ASC"},
            filter: {"@ENTITY_TYPE_ID": [1, 2, 7, 31]}    // Leads, deals, quotes, invoices.
        },
        function (result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.dir(result.data());
                if (result.more())
                    result.next();
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.link.list',
        [
            'order' => ['ENTITY_ID' => 'ASC'],
            'filter' => ['@ENTITY_TYPE_ID' => [1, 2, 7, 31]]
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
        bitrix_response = client.crm.requisite.link.list(
            order={"ENTITY_ID": "ASC"},
            filter={"@ENTITY_TYPE_ID": [1, 2, 7, 31]},
            start=0,
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

    Example `as_list`

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.requisite.link.list(
            order={"ENTITY_ID": "ASC"},
            filter={"@ENTITY_TYPE_ID": [1, 2, 7, 31]},
        ).as_list().response
        result = bitrix_response.result
        for item in result:
            print(item)
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

    Example `as_list_fast`

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.requisite.link.list(
            order={"ENTITY_ID": "ASC"},
            filter={"@ENTITY_TYPE_ID": [1, 2, 7, 31]},
        ).as_list_fast(descending=True).response
        result = bitrix_response.result
        for item in result:
            print(item)
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
    "result": [
        {
            "ENTITY_TYPE_ID": "7",
            "ENTITY_ID": "1",
            "REQUISITE_ID": "0",
            "BANK_DETAIL_ID": "0",
            "MC_REQUISITE_ID": "0",
            "MC_BANK_DETAIL_ID": "0"
        },
        {
            "ENTITY_TYPE_ID": "7",
            "ENTITY_ID": "2",
            "REQUISITE_ID": "0",
            "BANK_DETAIL_ID": "0",
            "MC_REQUISITE_ID": "0",
            "MC_BANK_DETAIL_ID": "0"
        },
        {
            "ENTITY_TYPE_ID": "7",
            "ENTITY_ID": "3",
            "REQUISITE_ID": "0",
            "BANK_DETAIL_ID": "0",
            "MC_REQUISITE_ID": "0",
            "MC_BANK_DETAIL_ID": "0"
        },
        {
            "ENTITY_TYPE_ID": "7",
            "ENTITY_ID": "4",
            "REQUISITE_ID": "7",
            "BANK_DETAIL_ID": "0",
            "MC_REQUISITE_ID": "2",
            "MC_BANK_DETAIL_ID": "2"
        },
        {
            "ENTITY_TYPE_ID": "7",
            "ENTITY_ID": "5",
            "REQUISITE_ID": "0",
            "BANK_DETAIL_ID": "0",
            "MC_REQUISITE_ID": "2",
            "MC_BANK_DETAIL_ID": "2"
        },
        {
            "ENTITY_TYPE_ID": "7",
            "ENTITY_ID": "6",
            "REQUISITE_ID": "0",
            "BANK_DETAIL_ID": "0",
            "MC_REQUISITE_ID": "2",
            "MC_BANK_DETAIL_ID": "2"
        },
        {
            "ENTITY_TYPE_ID": "2",
            "ENTITY_ID": "25",
            "REQUISITE_ID": "38",
            "BANK_DETAIL_ID": "0",
            "MC_REQUISITE_ID": "0",
            "MC_BANK_DETAIL_ID": "0"
        },
        {
            "ENTITY_TYPE_ID": "31",
            "ENTITY_ID": "315",
            "REQUISITE_ID": "60",
            "BANK_DETAIL_ID": "24",
            "MC_REQUISITE_ID": "2",
            "MC_BANK_DETAIL_ID": "2"
        }
    ],
    "total": 8,
    "time": {
        "start": 1718709631.410351,
        "finish": 1718709631.771324,
        "duration": 0.36097288131713867,
        "processing": 0.015230178833007812,
        "date_start": "2024-06-18T13:20:31+02:00",
        "date_finish": "2024-06-18T13:20:31+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md)| An array of objects with information from the selected requisite links. Each element contains the selected [requisite link fields](#fields) ||
|| **total**
[`integer`](../../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": 0,
    "error_description": "Access denied."
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `Access denied` | Insufficient access permissions to retrieve the list of requisite links ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-link-register.md)
- [{#T}](./crm-requisite-link-get.md)
- [{#T}](./crm-requisite-link-unregister.md)
- [{#T}](./crm-requisite-link-fields.md)