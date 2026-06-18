# Get a List of Requisites by Filter crm.requisite.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves a list of requisites based on a filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | An array containing the list of fields to be selected (see [requisite fields](./index.md#fields)).

If not provided or an empty array is passed, all available requisite fields will be selected. ||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the selected requisites in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to [requisite fields](./index.md#fields).

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:

- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as the value)
- `!@`— NOT IN (an array is passed as the value)
- `%` — LIKE, substring search. The `%` symbol does not need to be included in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol must be included in the value. Examples:
   - "mol%" — searching for values starting with "mol"
   - "%mol" — searching for values ending with "mol"
   - "%mol%" — searching for values where "mol" can be in any position.

- `%=` — LIKE (see description above)

- `!%` — NOT LIKE, substring search. The `%` symbol does not need to be included in the filter value. The search is performed from both sides.

- `!=%` — NOT LIKE, substring search. The `%` symbol must be included in the value. Examples:
   - "mol%" — searching for values not starting with "mol"
   - "%mol" — searching for values not ending with "mol"
   - "%mol%" — searching for values where the substring "mol" is not present in any position.

- `!%=` — NOT LIKE (see description above)

- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal ||
  || **order**
  [`object`](../../../data-types.md) | An object for sorting the selected requisites in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to [requisite fields](./index.md#fields).

Possible values for `order`:

- `asc` — ascending order
- `desc` — descending order
  ||
  || **start**
  [`integer`](../../../data-types.md) | This parameter is used for pagination control.

The page size of results is always static: 50 records.

To select the second page of results, the value `50` must be passed. To select the third page of results, the value is `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number
||
|#

## Code Example

{% include [Example Notes](../../../../_includes/examples.md) %}

1. Retrieving requisites by template ID

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"order":{"DATE_CREATE":"ASC"},"filter":{"PRESET_ID":"1"},"select":["ENTITY_TYPE_ID","ENTITY_ID","ID","NAME"]}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.list
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"order":{"DATE_CREATE":"ASC"},"filter":{"PRESET_ID":"1"},"select":["ENTITY_TYPE_ID","ENTITY_ID","ID","NAME"],"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.requisite.list
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        // Shape of each RequisiteResult returned in result[]
        type RequisiteResult = {
          ENTITY_TYPE_ID: string
          ENTITY_ID: string
          ID: string
          NAME: string
        }

        try {
          // crm.requisite.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make<RequisiteResult[]>({
            method: 'crm.requisite.list',
            params: {
              order: { DATE_CREATE: 'ASC' },
              filter: { PRESET_ID: '1' },
              select: ['ENTITY_TYPE_ID', 'ENTITY_ID', 'ID', 'NAME'],
              start: 0,
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info('Requisites:', result.length, result)
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
          async function getRequisiteList() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              // crm.requisite.list returns a single page (max 50 records). For the whole result set
              // use a list helper: $b24.actions.v2.callList.make() returns every record as one
              // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
              // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
              // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
              const response = await $b24.actions.v2.call.make({
                method: 'crm.requisite.list',
                params: {
                  order: { DATE_CREATE: 'ASC' },
                  filter: { PRESET_ID: '1' },
                  select: ['ENTITY_TYPE_ID', 'ENTITY_ID', 'ID', 'NAME'],
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
              console.info('Requisites:', result.length, result)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', getRequisiteList)
        </script>
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.requisite.list',
            [
                'order' => ["DATE_CREATE" => "ASC"],
                'filter' => ["PRESET_ID" => "1"],
                'select' => ["ENTITY_TYPE_ID", "ENTITY_ID", "ID", "NAME"]
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}

2. Retrieving the value of a custom field in requisites

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"order":{},"filter":{"ID":"51"},"select":["UF_CRM_1707997209"]}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.list
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"order":{},"filter":{"ID":"51"},"select":["UF_CRM_1707997209"],"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.requisite.list
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        // Shape of each RequisiteUserField returned in result[]
        type RequisiteUserField = {
          UF_CRM_1707997209: string
        }

        try {
          // crm.requisite.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make<RequisiteUserField[]>({
            method: 'crm.requisite.list',
            params: {
              order: {},
              filter: { ID: '51' }, // Requisite ID
              select: ['UF_CRM_1707997209'], // User-defined field ID
              start: 0,
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info('User field value:', result[0]?.UF_CRM_1707997209)
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
          async function getRequisiteUserField() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              // crm.requisite.list returns a single page (max 50 records). For the whole result set
              // use a list helper: $b24.actions.v2.callList.make() returns every record as one
              // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
              // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
              // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
              const response = await $b24.actions.v2.call.make({
                method: 'crm.requisite.list',
                params: {
                  order: {},
                  filter: { ID: '51' }, // Requisite ID
                  select: ['UF_CRM_1707997209'], // User-defined field ID
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
              console.info('User field value:', result[0]?.UF_CRM_1707997209)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', getRequisiteUserField)
        </script>
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.requisite.list',
            [
                'order' => [],
                'filter' => ['ID' => '51'],
                'select' => ['UF_CRM_1707997209']
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}

## Successful Response

HTTP status: **200**

1. Response from example 1:

    ```json
    {
    "result": [
        {
        "ENTITY_TYPE_ID": "4",
        "ENTITY_ID": "3027",
        "ID": "40",
        "NAME": "Organization"
        },
        {
        "ENTITY_TYPE_ID": "4",
        "ENTITY_ID": "3028",
        "ID": "41",
        "NAME": "Head Office Requisites"
        },
        {
        "ENTITY_TYPE_ID": "4",
        "ENTITY_ID": "3028",
        "ID": "42",
        "NAME": "Branch in Chernyakhovsk"
        }
    ],
    "total": 3,
    "time": {
        "start": 1717150154.197056,
        "finish": 1717150154.505106,
        "duration": 0.30804991722106934,
        "processing": 0.030454158782958984,
        "date_start": "2024-05-31T12:09:14+02:00",
        "date_finish": "2024-05-31T12:09:14+02:00",
        "operating": 0
    }
    }
    ```

2. Response from example 2:

    ```json
    {
        "result": [
            {
                "UF_CRM_1707997209": "45"
            }
        ],
        "total": 1,
        "time": {
            "start": 1717151052.551011,
            "finish": 1717151052.94743,
            "duration": 0.39641880989074707,
            "processing": 0.028468847274780273,
            "date_start": "2024-05-31T12:24:12+02:00",
            "date_finish": "2024-05-31T12:24:12+02:00",
            "operating": 0
        }
    }
    ```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md)| An array of objects containing information from the selected requisites. Each element contains the selected [requisite fields](./index.md#fields) ||
|| **total**
[`integer`](../../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Response

HTTP status: **400**

```json
{
    "error": 0,
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Code** | **Error Text** | **Description** ||
|| `0` | Access denied. | Insufficient access permissions to retrieve the list of requisites. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-requisite-add.md)
- [{#T}](./crm-requisite-update.md)
- [{#T}](./crm-requisite-get.md)
- [{#T}](./crm-requisite-delete.md)
- [{#T}](./crm-requisite-fields.md)