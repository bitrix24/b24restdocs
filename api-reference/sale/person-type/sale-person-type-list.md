# Get a list of payer types sale.persontype.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of payer types.

Use the `sale.persontype.list` method when creating a payment system **for orders**. In CRM (for old invoices, deals), the list of payer types is retrieved using the [crm.persontype.list](../../crm/outdated/invoice/crm-person-type-list.md) method.

## Method Parameters

#|
|| **Parameter**
`type`| **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to select (see fields of the [sale_person_type](../data-types.md#sale_person_type) object)

If not provided or an empty array is passed, all available fields of payer types will be selected
 ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected payer types in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [sale_person_type](../data-types.md#sale_person_type) object.

An additional prefix can be assigned to the key to clarify the filter behavior. Possible prefix values:
- `=` — equals (works with arrays as well)
- `%` — LIKE, substring search. The % symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string
- `>` — greater than
- `<` — less than
- `!=` — not equal
- `!%` — NOT LIKE, substring search. The % symbol in the filter value does not need to be passed. The search goes from both sides.
- `>=` — greater than or equal to
- `<=` — less than or equal to
- `=%` — LIKE, substring search. The % symbol needs to be passed in the value. Examples: 
    - `"mol%"` — searching for values starting with "mol"
    - `"%mol"` — searching for values ending with "mol"
    - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!=%` — NOT LIKE, substring search. The % symbol needs to be passed in the value. Examples:
    - `"mol%"` — searching for values not starting with "mol"
    - `"%mol"` — searching for values not ending with "mol"
    - `"%mol%"` — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (see description above)
 ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected payer types in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [sale_person_type](../data-types.md#sale_person_type) object

Possible values for order:

- `asc` — in ascending order
- `desc` — in descending order
 ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to control pagination.
 
The page size of results is always static: 50 records.
 
To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.
 
The formula for calculating the `start` parameter value:
 
`start = (N-1) * 50`, where `N` — the desired page number
 ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id", "name", "sort"],"filter":{"<=sort":100},"order":{"sort":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.persontype.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id", "name", "sort"],"filter":{"<=sort":100},"order":{"sort":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.persontype.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PersonTypeResult = {
      personTypes: Array<{
        id: number
        name: string
        sort: string
      }>
    }

    try {
      // sale.persontype.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<PersonTypeResult>({
        method: 'sale.persontype.list',
        params: {
          select: ['id', 'name', 'sort'],
          filter: { '<=sort': 100 },
          order: { sort: 'DESC' },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Person types:', result.personTypes, 'Count:', result.personTypes.length)
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
      async function fetchPersonTypeList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // sale.persontype.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sale.persontype.list',
            params: {
              select: ['id', 'name', 'sort'],
              filter: { '<=sort': 100 },
              order: { sort: 'DESC' },
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
          console.info('Person types:', result.personTypes, 'Count:', result.personTypes.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchPersonTypeList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.persontype.list',
                [
                    'select' => ["id", "name", "sort"],
                    'filter' => ['<=sort' => 100],
                    'order'  => ['sort' => 'DESC']
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
        echo 'Error fetching person types: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.persontype.list',
        {
            select: ["id", "name", "sort"],
            filter: {'<=sort': 100},
            order: {'sort': 'DESC'}
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.persontype.list',
        [
            'select' => ["id", "name", "sort"],
            'filter' => ['<=sort' => 100],
            'order' => ['sort' => 'DESC']
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
    "personTypes": [
        {	
            "id": 4,
            "name": "Legal Entity",
            "sort": "110"
        },
        {
            "id": 3,
            "name": "Individual",
            "sort": "100"
        },
        {
            "id": 12,
            "name": "Legal Entity",
            "sort": "100"
        }
    ],
    "total": 3,
    "time": {
        "start": 1712326352.63409,
        "finish": 1712326352.8319,
        "duration": 0.197818040847778,
        "processing": 0.00833678245544434,
        "date_start": "2024-04-05T16:12:32+02:00",
        "date_finish": "2024-04-05T16:12:32+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **personTypes**
[`sale_person_type[]`](../../data-types.md) | Array of objects with information about the selected payer types ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 200040300010,
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | No access to read ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-person-type-add.md)
- [{#T}](./sale-person-type-update.md)
- [{#T}](./sale-person-type-get.md)
- [{#T}](./sale-person-type-delete.md)
- [{#T}](./sale-person-type-get-fields.md)

