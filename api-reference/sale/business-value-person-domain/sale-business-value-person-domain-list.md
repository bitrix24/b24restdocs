# Get a list of matches for a natural or legal person sale.businessValuePersonDomain.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.businessValuePersonDomain.list` retrieves a list of matches for a natural or legal person.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | The array contains a list of fields to select.

If not provided or an empty array is passed, all available fields for matching a natural or legal person will be selected.

Possible values for array elements:
- `personTypeId`
- `domain` ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected matches for a natural or legal person in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field`:
- `personTypeId`
- `domain` 

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as the value)
- `!@`— NOT IN (an array is passed as the value)
- `=` — equals, exact match (used by default)
- `!=` - not equal ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected matches for a natural or legal person in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field`:
- `personTypeId`
- `domain` 

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order ||
|| **start**
[`integer`](../../data-types.md) | The parameter is used to manage pagination.

The page size of results is always static: 50 records.

To select the second page of results, the value `50` must be passed. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["personTypeId"],"filter":{"=domain":"I"},"order":{"personTypeId":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.businessValuePersonDomain.list
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["personTypeId"],"filter":{"=domain":"I"},"order":{"personTypeId":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.businessValuePersonDomain.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type BusinessValuePersonDomain = {
      personTypeId: number
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type BusinessValuePersonDomainListResult = {
      businessValuePersonDomains: BusinessValuePersonDomain[]
    }

    // sale.businessValuePersonDomain.list returns a single page (max 50 records). For the whole result set
    // use a list helper: $b24.actions.v2.callList.make() returns every record as one
    // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
    // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
    // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
    try {
      const response = await $b24.actions.v2.call.make<BusinessValuePersonDomainListResult>({
        method: 'sale.businessValuePersonDomain.list',
        params: {
          select: ['personTypeId'],
          filter: { '=domain': 'I' },
          order: { personTypeId: 'DESC' },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.businessValuePersonDomains.length, result.businessValuePersonDomains)
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
      async function fetchBusinessValuePersonDomainList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // sale.businessValuePersonDomain.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'sale.businessValuePersonDomain.list',
            params: {
              select: ['personTypeId'],
              filter: { '=domain': 'I' },
              order: { personTypeId: 'DESC' },
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
          console.info(result.businessValuePersonDomains.length, result.businessValuePersonDomains)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchBusinessValuePersonDomainList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.businessValuePersonDomain.list',
                [
                    'select' => ["personTypeId"],
                    'filter' => ["=domain" => "I"],
                    'order' => ["personTypeId" => "DESC"]
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
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.businessValuePersonDomain.list',
        {
            select: ["personTypeId"],
            filter: {"=domain": "I"},
            order: {"personTypeId": "DESC"}
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
        'sale.businessValuePersonDomain.list',
        [
            'select' => ['personTypeId'],
            'filter' => ['=domain' => 'I'],
            'order' => ['personTypeId' => 'DESC']
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
    "result": {
        "businessValuePersonDomains": [
            {
                "personTypeId": 3
            },
            {
                "personTypeId": 2
            }
        ]
    },
    "total": 2,
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
[`object`](../../data-types.md) | The root element of the response ||
|| **businessValuePersonDomains**
[`sale_business_value_person_domain[]`](../data-types.md) | An array of objects with information about the selected matches ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":200040300010,
    "error_description":"Access Denied"
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

- [{#T}](./sale-business-value-person-domain-add.md)
- [{#T}](./sale-business-value-person-domain-delete-by-filter.md)
- [{#T}](./sale-business-value-person-domain-get-fields.md)

