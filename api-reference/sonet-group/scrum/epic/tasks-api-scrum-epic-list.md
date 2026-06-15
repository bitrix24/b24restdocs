# Get a List of Epics tasks.api.scrum.epic.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method returns a list of epics.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **order**
[`array`](../../../data-types.md) | An array for sorting the result in the format `{'sort_field': 'sort_direction' [, ...]}`. 

The sort direction can take the following values:
- `asc` — ascending
- `desc` — descending

Possible values for the array elements correspond to the fields in the response of [tasks.api.scrum.epic.add](./tasks-api-scrum-epic-add.md#fields)
||
|| **filter**
[`array`](../../../data-types.md) | An array in the format `{'filter_field': 'filter_value' [, ...]}`.

An additional prefix can be specified for the key to clarify the filter behavior.

Possible prefix values:
- `=` — equals (works with arrays as well)
- `%` — LIKE, substring search. The `%` symbol does not need to be included in the filter value. The search looks for the substring in any position of the string.
- `>` — greater than
- `<` — less than
- `!=` — not equal
- `!%` — NOT LIKE, substring search. The `%` symbol does not need to be included in the filter value. The search goes from both sides.
- `>=` — greater than or equal to
- `<=` — less than or equal to
- `=%` — LIKE, substring search. The `%` symbol must be included in the value. Examples:
  - `"mol%"` — searching for values starting with "mol"
  - `"%mol"` — searching for values ending with "mol"
  - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!=%` — NOT LIKE, substring search. The `%` symbol must be included in the value. Examples:
  - `"mol%"` — searching for values not starting with "mol"
  - `"%mol"` — searching for values not ending with "mol"
  - `"%mol%"` — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (see description above)

Possible values for the array elements correspond to the fields in the response of [tasks.api.scrum.epic.add](./tasks-api-scrum-epic-add.md#fields)

||
|| **select**
[`array`](../../../data-types.md) | An array of record fields that will be returned by the method.

Possible values for the array elements correspond to the fields in the response of [tasks.api.scrum.epic.add](./tasks-api-scrum-epic-add.md#fields). You can specify only the fields that are necessary.

If the array contains the value `"*"`, all available fields will be returned.

The default value is an empty array `array()`. This means that all fields from the main query table will be returned.
||
|| **start**
[`integer`](../../../data-types.md) | The page number of the output. Works for HTTPS requests.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.

The formula for calculating the `start` parameter value:
`start = (N-1) * 50`, where `N` is the desired page number.
||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
        "filter": {
            "GROUP_ID": 143,
            ">=ID": 1,
            "<=ID": 50,
            "NAME": "%epic%",
            "!=DESCRIPTION": "old epic"
        },
        "order": {
            "ID": "asc",
            "NAME": "desc"
        },
        "select": ["ID", "NAME", "DESCRIPTION", "CREATED_BY", "MODIFIED_BY", "COLOR"],
        "start": 0
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.epic.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
        "filter": {
            "GROUP_ID": 143,
            ">=ID": 1,
            "<=ID": 50,
            "NAME": "%epic%",
            "!=DESCRIPTION": "old epic",
            "CREATED_BY": 1,
            "MODIFIED_BY": 3,
            "COLOR": "#69dafc"
        },
        "order": {
            "ID": "asc",
            "NAME": "desc"
        },
        "select": ["ID", "NAME", "DESCRIPTION", "CREATED_BY", "MODIFIED_BY", "COLOR"],
        "start": 0,
        "auth": "YOUR_ACCESS_TOKEN"
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.epic.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each EpicItem returned in result[]
    type EpicItem = {
      id: number
      groupId: number
      name: string
      description: string
      createdBy: number
      modifiedBy: number
      color: string
    }

    try {
      // tasks.api.scrum.epic.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<EpicItem[]>({
        method: 'tasks.api.scrum.epic.list',
        params: {
          filter: {
            GROUP_ID: 143,
            '>=ID': 1,
            '<=ID': 50,
            NAME: '%epic%',
            '!=DESCRIPTION': 'old epic',
            CREATED_BY: 1,
            MODIFIED_BY: 3,
            COLOR: '#69dafc',
          },
          order: {
            ID: 'asc',
            NAME: 'desc',
          },
          select: ['ID', 'NAME', 'DESCRIPTION', 'CREATED_BY', 'MODIFIED_BY', 'COLOR'],
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Loaded epics:', result.length, result)
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
      async function loadEpicList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // tasks.api.scrum.epic.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'tasks.api.scrum.epic.list',
            params: {
              filter: {
                GROUP_ID: 143,
                '>=ID': 1,
                '<=ID': 50,
                NAME: '%epic%',
                '!=DESCRIPTION': 'old epic',
                CREATED_BY: 1,
                MODIFIED_BY: 3,
                COLOR: '#69dafc',
              },
              order: {
                ID: 'asc',
                NAME: 'desc',
              },
              select: ['ID', 'NAME', 'DESCRIPTION', 'CREATED_BY', 'MODIFIED_BY', 'COLOR'],
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
          console.info('Loaded epics:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', loadEpicList)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.api.scrum.epic.list',
                [
                    'filter' => [
                        'GROUP_ID'      => $groupId,
                        '>=ID'          => 1,
                        '<=ID'          => 50,
                        'NAME'          => '%epic%',
                        '!=DESCRIPTION' => 'old epic',
                        'CREATED_BY'    => 1,
                        'MODIFIED_BY'   => 3,
                        'COLOR'         => '#69dafc'
                    ],
                    'order'  => [
                        'ID'   => 'asc',
                        'NAME' => 'desc'
                    ],
                    'select' => ['ID', 'NAME', 'DESCRIPTION', 'CREATED_BY', 'MODIFIED_BY', 'COLOR'],
                    'start'  => 0
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const groupId = 143;
    BX24.callMethod(
        'tasks.api.scrum.epic.list',
        {
            filter: {
                GROUP_ID: groupId,
                '>=ID': 1,
                '<=ID': 50,
                'NAME': '%epic%',
                '!=DESCRIPTION': 'old epic',
                'CREATED_BY': 1,
                'MODIFIED_BY': 3,
                'COLOR': '#69dafc'
            },
            order: {
                'ID': 'asc',
                'NAME': 'desc'
            },
            select: ['ID', 'NAME', 'DESCRIPTION', 'CREATED_BY', 'MODIFIED_BY', 'COLOR']
        },
        function(res)
        {
            if (res.error())
            {
                console.error(res.error());
                return;
            }

            console.log(res.data());

            if (res.more())
            {
                res.next();
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    // executing a request to the REST API
    $result = CRest::call(
        'tasks.api.scrum.epic.list',
        [
            'filter' => [
                'GROUP_ID' => 143,
                '>=ID' => 1,
                '<=ID' => 50,
                'NAME' => '%epic%',
                '!=DESCRIPTION' => 'old epic',
                'CREATED_BY' => 1,
                'MODIFIED_BY' => 3,
                'COLOR' => '#69dafc'
            ],
            'order' => [
                'ID' => 'asc',
                'NAME' => 'desc'
            ],
            'select' => ['ID', 'NAME', 'DESCRIPTION', 'CREATED_BY', 'MODIFIED_BY', 'COLOR'],
            'start' => 0
        ]
    );

    // Processing the response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    }
    else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
[
    {
        "id": 1,
        "groupId": 143,
        "name": "epic",
        "description": "",
        "createdBy": 1,
        "modifiedBy": 0,
        "color": "#69dafc"
    },
    {
        "id": 3,
        "groupId": 143,
        "name": "epic2",
        "description": "new epic",
        "createdBy": 3,
        "modifiedBy": 5,
        "color": "#69dagc"
    }
]
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Epic identifier ||
|| **groupId**
[`integer`](../../../data-types.md) | Identifier of the group (scrum) to which the epic is linked ||
|| **name**
[`string`](../../../data-types.md) | Name of the epic ||
|| **description**
[`string`](../../../data-types.md) | Description of the epic ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the user who created the epic ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | Identifier of the user who last modified the epic ||
|| **color**
[`string`](../../../data-types.md) | Color of the epic in HEX format ||

|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 0,
    "error_description": "Could not load list"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description**  | **Value** ||
|| `0` | Could not load list| No epics found with the specified filters ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-epic-add.md)
- [{#T}](./tasks-api-scrum-epic-update.md)
- [{#T}](./tasks-api-scrum-epic-get.md)
- [{#T}](./tasks-api-scrum-epic-delete.md)
- [{#T}](./tasks-api-scrum-epic-get-fields.md)