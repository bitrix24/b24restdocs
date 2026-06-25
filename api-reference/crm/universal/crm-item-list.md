# Get a List of CRM Items: crm.item.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with "read" access permission for CRM object elements

This method retrieves a list of items of a specific type from the CRM object.

CRM object items will not be included in the final selection if the user does not have "read" access permission for those items.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`][1] | Identifier of the [system](../data-types.md#object_type) or [custom type](./user-defined-object-types/index.md) whose items need to be retrieved.

Numerical values for system types (Lead — 1, Deal — 2, Contact — 3, Company — 4, Invoice — 31, etc.) are listed in the [CRM object types reference](../data-types.md#object_type). The identifier of the smart process can be obtained using the [crm.type.list](./user-defined-object-types/crm-type-list.md) method. ||
|| **select**
[`array`][1] | List of fields that must be filled for items in the selection.

It can contain only item field names or `'*'`.

The list of all available fields for selection can be found using the [`crm.item.fields`](./crm-item-fields.md) method. The list of standard fields is available in the article [CRM object fields](./object-fields.md)

The `fm` field (multiple fields: phones, e-mail, messengers) is not a CRM object field and cannot be explicitly requested via `select`. To receive `fm` in the response, pass `select: ['*']`
||
|| **filter**
[`object`][1] |
Object format:
```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```
where
- `field_n` — name of the field by which the selection of items will be filtered
- `value_n` — filter value

The filter can have unlimited nesting and number of conditions.
By default, all conditions are combined with `AND`. If you need to use `OR`, you can pass a special key `logic` with the value `OR`.

You can add a prefix to the `field_n` keys to specify the filter operation.
Possible prefix values:
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

A list of all available fields for filtering can be obtained using the [`crm.item.fields`](./crm-item-fields.md) method. A list of standard fields is available in the article [CRM Object Fields](./object-fields.md).
||
|| **order**
[`object`][1] |
Object format:
```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```
where
- `field_n` — the name of the field by which the items selection will be sorted
- `value_n` — a value of type `string` equal to:
  - `ASC` — sorting in ascending order
  - `DESC` — sorting in descending order

The list of all available fields for sorting can be found using the [`crm.item.fields`](./crm-item-fields.md) method. The list of standard fields is available in the article [CRM object fields](./object-fields.md)
||
|| **start**
[`integer`][1] | This parameter is used to control pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page
||
|| **useOriginalUfNames**
[`boolean`][1] | Parameter to control the format of custom field names in the request and response.   
Possible values:

- `Y` — original names of custom fields, e.g., `UF_CRM_2_1639669411830`
- `N` — custom field names in camelCase, e.g., `ufCrm2_1639669411830`

Default is `N`. ||
|#

## Code Examples

Retrieve a list of leads where:
1. First name or last name is not empty.
2. They are in the status "In Progress" or "Unprocessed".
3. They came from sources "Ads" or "Sites".
4. They are assigned to managers with IDs 1 or 6.
5. They have a deal amount between 5000 and 20000.
6. The calculation mode for the amount is manual.

Set the following sorting order for this selection:
* First name and last name in ascending order.

For clarity, we will select only the fields we need:
* Identifier `id`
* Title `title`
* First name `name`
* Last name `lastName`
* Stage ID `stageId`
* Source ID `sourceId`
* Responsible ID `assignedById`
* Amount `opportunity`
* Manual calculation mode `isManualOpportunity`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1,"select":["id","title","lastName","name","stageId","sourceId","assignedById","opportunity","isManualOpportunity"],"filter":{"0":{"logic":"OR","0":{"!=name":""},"1":{"!=lastName":""}},"@stageId":["NEW","IN_PROCESS"],"@sourceId":["WEB","ADVERTISING"],"@assignedById":[1,6],">=opportunity":5000,"<=opportunity":20000,"isManualOpportunity":"Y"},"order":{"lastName":"ASC","name":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1,"select":["id","title","lastName","name","stageId","sourceId","assignedById","opportunity","isManualOpportunity"],"filter":{"0":{"logic":"OR","0":{"!=name":""},"1":{"!=lastName":""}},"@stageId":["NEW","IN_PROCESS"],"@sourceId":["WEB","ADVERTISING"],"@assignedById":[1,6],">=opportunity":5000,"<=opportunity":20000,"isManualOpportunity":"Y"},"order":{"lastName":"ASC","name":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CrmItem = {
      id: number
      assignedById: number
      stageId: string
      opportunity: number
      sourceId: string
      title: string
      name: string
      lastName: string | null
      isManualOpportunity: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CrmItemListResult = {
      items: CrmItem[]
    }

    try {
      // crm.item.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<CrmItemListResult>({
        method: 'crm.item.list',
        params: {
          entityTypeId: 1,
          select: [
            'id',
            'title',
            'lastName',
            'name',
            'stageId',
            'sourceId',
            'assignedById',
            'opportunity',
            'isManualOpportunity',
          ],
          filter: {
            '0': {
              logic: 'OR',
              '0': {
                '!=name': '',
              },
              '1': {
                '!=lastName': '',
              },
            },
            '@stageId': ['NEW', 'IN_PROCESS'],
            '@sourceId': ['WEB', 'ADVERTISING'],
            '@assignedById': [1, 6],
            '>=opportunity': 5000,
            '<=opportunity': 20000,
            isManualOpportunity: 'Y',
          },
          order: {
            lastName: 'ASC',
            name: 'ASC',
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Fetched items:', result.items.length, result.items)
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
      async function getCrmItemList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.item.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.list',
            params: {
              entityTypeId: 1,
              select: [
                'id',
                'title',
                'lastName',
                'name',
                'stageId',
                'sourceId',
                'assignedById',
                'opportunity',
                'isManualOpportunity',
              ],
              filter: {
                '0': {
                  logic: 'OR',
                  '0': {
                    '!=name': '',
                  },
                  '1': {
                    '!=lastName': '',
                  },
                },
                '@stageId': ['NEW', 'IN_PROCESS'],
                '@sourceId': ['WEB', 'ADVERTISING'],
                '@assignedById': [1, 6],
                '>=opportunity': 5000,
                '<=opportunity': 20000,
                isManualOpportunity: 'Y',
              },
              order: {
                lastName: 'ASC',
                name: 'ASC',
              },
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
          console.info('Fetched items:', result.items.length, result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCrmItemList)
    </script>
    ```

- PHP

    ```php        
    try {
        $entityTypeId = 1; // Replace with actual entity type ID
        $order = []; // Replace with actual order array
        $filter = []; // Replace with actual filter array
        $select = []; // Replace with actual select array
        $startItem = 0; // Optional, can be adjusted as needed
        $itemsResult = $serviceBuilder
            ->getCRMScope()
            ->item()
            ->list($entityTypeId, $order, $filter, $select, $startItem);
        foreach ($itemsResult->getItems() as $item) {
            print("ID: " . $item->id . PHP_EOL);
            print("XML ID: " . $item->xmlId . PHP_EOL);
            print("Title: " . $item->title . PHP_EOL);
            print("Created By: " . $item->createdBy . PHP_EOL);
            print("Updated By: " . $item->updatedBy . PHP_EOL);
            print("Created Time: " . $item->createdTime->format(DATE_ATOM) . PHP_EOL);
            print("Updated Time: " . $item->updatedTime->format(DATE_ATOM) . PHP_EOL);
            // Add more fields as necessary
        }
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage() . PHP_EOL);
    }
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.item.list(
            entity_type_id=1,
            select=[
                "id",
                "title",
                "lastName",
                "name",
                "stageId",
                "sourceId",
                "assignedById",
                "opportunity",
                "isManualOpportunity",
            ],
            filter={
                "0": {
                    "logic": "OR",
                    "0": {
                        "!=name": "",
                    },
                    "1": {
                        "!=lastName": "",
                    },
                },
                "@stageId": ["NEW", "IN_PROCESS"],
                "@sourceId": ["WEB", "ADVERTISING"],
                "@assignedById": [1, 6],
                ">=opportunity": 5000,
                "<=opportunity": 20000,
                "isManualOpportunity": "Y",
            },
            order={
                "lastName": "ASC",
                "name": "ASC",
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

    Example `as_list`

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.item.list(
            entity_type_id=1,
            select=[
                "id",
                "title",
                "lastName",
                "name",
                "stageId",
                "sourceId",
                "assignedById",
                "opportunity",
                "isManualOpportunity",
            ],
            filter={
                "0": {
                    "logic": "OR",
                    "0": {
                        "!=name": "",
                    },
                    "1": {
                        "!=lastName": "",
                    },
                },
                "@stageId": ["NEW", "IN_PROCESS"],
                "@sourceId": ["WEB", "ADVERTISING"],
                "@assignedById": [1, 6],
                ">=opportunity": 5000,
                "<=opportunity": 20000,
                "isManualOpportunity": "Y",
            },
            order={
                "lastName": "ASC",
                "name": "ASC",
            },
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
        bitrix_response = client.crm.item.list(
            entity_type_id=1,
            select=[
                "id",
                "title",
                "lastName",
                "name",
                "stageId",
                "sourceId",
                "assignedById",
                "opportunity",
                "isManualOpportunity",
            ],
            filter={
                "0": {
                    "logic": "OR",
                    "0": {
                        "!=name": "",
                    },
                    "1": {
                        "!=lastName": "",
                    },
                },
                "@stageId": ["NEW", "IN_PROCESS"],
                "@sourceId": ["WEB", "ADVERTISING"],
                "@assignedById": [1, 6],
                ">=opportunity": 5000,
                "<=opportunity": 20000,
                "isManualOpportunity": "Y",
            },
            order={
                "lastName": "ASC",
                "name": "ASC",
            },
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

- BX24.js

    ```js
        BX24.callMethod(
            'crm.item.list',
            {
                entityTypeId: 1,
                select: [
                    "id", 
                    "title",
                    "lastName",
                    "name",
                    "stageId", 
                    "sourceId", 
                    "assignedById", 
                    "opportunity", 
                    "isManualOpportunity",
                ],
                filter: {
                    "0": {
                        logic: "OR",
                        "0": {
                            "!=name": "",
                        },
                        "1": {
                            "!=lastName": "",
                        },
                    },
                    "@stageId": ["NEW", "IN_PROCESS"],
                    "@sourceId": ['WEB', "ADVERTISING"],
                    "@assignedById": [1, 6],
                    ">=opportunity": 5000,
                    "<=opportunity": 20000,
                    "isManualOpportunity": "Y",
                },
                order: {
                    lastName: 'ASC',
                    name: 'ASC',
                },
            },
            (result) => {
                if (result.error())
                {
                    console.error(result.error());

                    return;
                }

                console.info(result.data());
            },
        );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.list',
        [
            'entityTypeId' => 1,
            'select' => [
                "id",
                "title",
                "lastName",
                "name",
                "stageId",
                "sourceId",
                "assignedById",
                "opportunity",
                "isManualOpportunity",
            ],
            'filter' => [
                "0" => [
                    "logic" => "OR",
                    "0" => [
                        "!=name" => "",
                    ],
                    "1" => [
                        "!=lastName" => "",
                    ],
                ],
                "@stageId" => ["NEW", "IN_PROCESS"],
                "@sourceId" => ['WEB', "ADVERTISING"],
                "@assignedById" => [1, 6],
                ">=opportunity" => 5000,
                "<=opportunity" => 20000,
                "isManualOpportunity" => "Y",
            ],
            'order' => [
                'lastName' => 'ASC',
                'name' => 'ASC',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

### Example Request with Date Filter Using OR Logic

Filter deals `entityTypeId = 2` by two creation dates. For each date, set the start and end of the day range.

For clarity, we will select only the fields we need:
* Identifier `id`
* Title `title`
* Create date `createdTime`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"select":["id","title","createdTime"],"filter":{"0":{"logic":"OR","0":{">=createdTime":"2025-10-31T00:00:00+02:00","<createdTime":"2025-11-01T00:00:00+02:00"},"1":{">=createdTime":"2025-02-28T00:00:00+02:00","<createdTime":"2025-03-01T00:00:00+02:00"}}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"select":["id","title","createdTime"],"filter":{"0":{"logic":"OR","0":{">=createdTime":"2025-10-31T00:00:00+02:00","<createdTime":"2025-11-01T00:00:00+02:00"},"1":{">=createdTime":"2025-02-28T00:00:00+02:00","<createdTime":"2025-03-01T00:00:00+02:00"}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CrmItem = {
      id: number
      title: string
      createdTime: ISODate | null
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CrmItemListResult = {
      items: CrmItem[]
    }

    try {
      // crm.item.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<CrmItemListResult>({
        method: 'crm.item.list',
        params: {
          entityTypeId: 2,
          select: ['id', 'title', 'createdTime'],
          filter: {
            '0': {
              logic: 'OR',
              '0': {
                '>=createdTime': '2025-10-31T00:00:00+02:00',
                '<createdTime': '2025-11-01T00:00:00+02:00',
              },
              '1': {
                '>=createdTime': '2025-02-28T00:00:00+02:00',
                '<createdTime': '2025-03-01T00:00:00+02:00',
              },
            },
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Fetched items:', result.items.length, result.items)
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
      async function getCrmItemListByDate() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.item.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.list',
            params: {
              entityTypeId: 2,
              select: ['id', 'title', 'createdTime'],
              filter: {
                '0': {
                  logic: 'OR',
                  '0': {
                    '>=createdTime': '2025-10-31T00:00:00+02:00',
                    '<createdTime': '2025-11-01T00:00:00+02:00',
                  },
                  '1': {
                    '>=createdTime': '2025-02-28T00:00:00+02:00',
                    '<createdTime': '2025-03-01T00:00:00+02:00',
                  },
                },
              },
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
          console.info('Fetched items:', result.items.length, result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCrmItemListByDate)
    </script>
    ```

- PHP

    ```php
    try {
        $entityTypeId = 2;
        $order = [];
        $filter = [
            "0" => [
                "logic" => "OR",
                "0" => [
                    ">=createdTime" => "2025-10-31T00:00:00+02:00",
                    "<createdTime" => "2025-11-01T00:00:00+02:00",
                ],
                "1" => [
                    ">=createdTime" => "2025-02-28T00:00:00+02:00",
                    "<createdTime" => "2025-03-01T00:00:00+02:00",
                ],
            ],
        ];
        $select = ['id', 'title', 'createdTime'];
        $startItem = 0;

        $itemsResult = $serviceBuilder
            ->getCRMScope()
            ->item()
            ->list($entityTypeId, $order, $filter, $select, $startItem);

        foreach ($itemsResult->getItems() as $item) {
            print("ID: " . $item->id . PHP_EOL);
            print("Title: " . $item->title . PHP_EOL);
            print("Created Time: " . $item->createdTime->format(DATE_ATOM) . PHP_EOL);
            print(PHP_EOL);
        }
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage() . PHP_EOL);
    }
    ```

- BX24.js

    ```js
        BX24.callMethod(
            'crm.item.list',
            {
                entityTypeId: 2,
                select: ['id', 'title', 'createdTime'],
                filter: {
                    '0': {
                        logic: 'OR',
                        '0': {
                            '>=createdTime': '2025-10-31T00:00:00+02:00',
                            '<createdTime': '2025-11-01T00:00:00+02:00',
                        },
                        '1': {
                            '>=createdTime': '2025-02-28T00:00:00+02:00',
                            '<createdTime': '2025-03-01T00:00:00+02:00',
                        },
                    },
                },
            },
            function (result) {
                if (result.error()) {
                    console.error('crm.item.list error', result.error());
                    return;
                }

                const { items } = result.data();
                items.forEach((item) => {
                    console.log(`Deal #${item.id}: ${item.title} (${item.createdTime})`);
                });

                if (result.more()) {
                    result.next();
                }
            }
        );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.list',
        [
            'entityTypeId' => 2,
            'select' => ['id', 'title', 'createdTime'],
            'filter' => [
                "0" => [
                    "logic" => "OR",
                    "0" => [
                        ">=createdTime" => "2025-10-31T00:00:00+02:00",
                        "<createdTime" => "2025-11-01T00:00:00+02:00",
                    ],
                    "1" => [
                        ">=createdTime" => "2025-02-28T00:00:00+02:00",
                        "<createdTime" => "2025-03-01T00:00:00+02:00",
                    ],
                ],
            ],
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
        "items": [
            {
                "id": 253,
                "assignedById": 6,
                "stageId": "NEW",
                "opportunity": 19000,
                "sourceId": "WEB",
                "title": "Lead #253",
                "name": "Admin",
                "lastName": null,
                "isManualOpportunity": "Y"
            },
            {
                "id": 255,
                "assignedById": 1,
                "stageId": "NEW",
                "opportunity": 19600,
                "sourceId": "WEB",
                "title": "Lead #255",
                "name": "John",
                "lastName": "Smith",
                "isManualOpportunity": "Y"
            },
            {
                "id": 252,
                "assignedById": 1,
                "stageId": "NEW",
                "opportunity": 12000,
                "sourceId": "ADVERTISING",
                "title": "Lead #252",
                "name": "John",
                "lastName": "Smith",
                "isManualOpportunity": "Y"
            },
            {
                "id": 254,
                "assignedById": 6,
                "stageId": "IN_PROCESS",
                "opportunity": 19000,
                "sourceId": "ADVERTISING",
                "title": "Lead #254",
                "name": "Smith",
                "lastName": "Smith",
                "isManualOpportunity": "Y"
            }
        ]
    },
    "total": 4,
    "time": {
        "start": 1721724354.214286,
        "finish": 1721724354.805263,
        "duration": 0.5909769535064697,
        "processing": 0.24513697624206543,
        "date_start": "2024-07-23T10:45:54+02:00",
        "date_finish": "2024-07-23T10:45:54+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`][1] | The root element of the response. Contains a single key `items` ||
|| **items**
[`item[]`](./object-fields.md) | Array with information about the found items.

Returned fields depend on the `select` parameter, [field descriptions](./object-fields.md). ||
|| **total**
[`integer`][1] | The total number of found items ||
|| **next**
[`integer`][1] | Contains the value to be passed in the next request in the `start` parameter to get the next batch of data.

The `next` parameter appears in the response if the number of items matching your request exceeds `50`. ||
|| **time**
[`time`][1] | Information about the request execution time ||
|#

{% note info " " %}

By default, custom field names are passed and returned in camelCase, for example `ufCrm2_1639669411830`.
When passing the parameter `useOriginalUfNames` with the value `Y`, custom fields will be returned with their original names, for example `UF_CRM_2_1639669411830`.

{% endnote %}

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "INVALID_ARG_VALUE",
    "error_description": "Invalid filter: field 'FIELD' is not allowed in filter"
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code**                          | **Description**                                             | **Value**                                          ||
|| `403`      | `allowed_only_intranet_user`     | Action is allowed for intranet users only         | User is not an intranet user       ||
|| `400`      | `NOT_FOUND`                      | SPA not found                                  | Occurs when an invalid `entityTypeId` is passed    ||
|| `400`      | `INVALID_ARG_VALUE`              | Invalid filter: field '`field`' is not allowed in filter | The field `filter` passed in `field` is not available for filtering ||
|| `400`      | `INVALID_ARG_VALUE`              | Invalid filter: field '`field`' has invalid value        | The value passed for field `field` in `filter` is incorrect ||
|| `400`      | `INVALID_ARG_VALUE`              | Invalid order: field '`field`' is not allowed in order   | The field `order` passed in `field` is not available for sorting ||
|| `400`      | `INVALID_ARG_VALUE`              | Invalid order: allowed sort directions are `ASC, DESC`. But got '`orderValue`' for field '`field`' | The value `orderValue` passed for field `field` in the `order` parameter is incorrect ||
|#

{% include [System errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](crm-item-add.md)
- [{#T}](crm-item-update.md)
- [{#T}](crm-item-get.md)
- [{#T}](crm-item-delete.md)
- [{#T}](crm-item-fields.md)
- [{#T}](./object-fields.md)
- [{#T}](../../../tutorials/tasks/how-to-connect-task-to-spa.md)
- [{#T}](../../../tutorials/crm/how-to-get-lists/how-to-get-elements-by-stage-filter.md)
- [{#T}](../../../tutorials/crm/how-to-get-lists/get-activity-list-by-deals.md)
- [{#T}](../../../tutorials/crm/how-to-get-lists/how-to-get-contractors.md)
  
[1]: ../../data-types.md