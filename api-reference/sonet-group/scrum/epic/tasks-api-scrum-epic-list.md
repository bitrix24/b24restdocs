# Get a List of Epics tasks.api.scrum.epic.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method returns a list of epics. The list includes only epics from the groups the user is a member of.

## Method Parameters

Pass epic field names in uppercase in `filter`, `select`, and `order`:

#|
|| **Response Field** | **Name in `filter`, `select`, and `order`** ||
|| `id` | `ID` ||
|| `groupId` | `GROUP_ID` ||
|| `name` | `NAME` ||
|| `description` | `DESCRIPTION` ||
|| `createdBy` | `CREATED_BY` ||
|| `modifiedBy` | `MODIFIED_BY` ||
|| `color` | `COLOR` ||
|#

{% note warning "Attention" %}

If you pass a field in `filter` in a different case, for example `groupId`, or a nonexistent field, the method returns an empty array without an error. The method responds the same way to a nonexistent field in `select` and `order`.

The method still returns fields not included in `select`, but with the value `0` or an empty string. Do not mistake these values for epic data

{% endnote %}

#|
|| **Name**
`type` | **Description** ||
|| **order**
[`object`](../../../data-types.md) | An object for sorting the result in the format `{"sort_field": "sort_direction" [, ...]}`.

The sort direction can take the following values:
- `asc` — ascending
- `desc` — descending
||
|| **filter**
[`object`](../../../data-types.md) | An object in the format `{"filter_field": "filter_value" [, ...]}`.

An additional prefix can be specified for the key to clarify the filter behavior.

Possible prefix values:
- `=` — equals (works with arrays as well)
- `%` — LIKE, substring search. The `%` symbol does not need to be included in the filter value. The search looks for the substring in any position of the string.
- `>` — greater than
- `<` — less than
- `!=` — not equal
- `!%` — NOT LIKE, substring search. The `%` symbol does not need to be included in the filter value. The search goes from both sides
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

If there is no prefix and the value contains the `%` character, the filter also searches by substring: `"NAME": "%epic%"`
||
|| **select**
[`array`](../../../data-types.md) | An array of fields to fill in the response, for example `["ID", "NAME"]`. The value `"*"` or an empty array means all fields ||
|| **start**
[`integer`](../../../data-types.md) | Selection offset, a multiple of 50, defaults to `0`. The page size of results is always 50 records: to retrieve the second page, pass `50`; for the third page, pass `100`.

Calculation formula: `start = (N-1) * 50`, where `N` is the desired page number. The method rounds a value that is not a multiple of 50 down to the start of the page: with `start: 2`, it returns the first page
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
      // tasks.api.scrum.epic.list returns a single page (max 50 records) without `total` and `next`.
      // To load the next page, repeat the call with `start` increased by 50.
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

          // tasks.api.scrum.epic.list returns a single page (max 50 records) without `total` and `next`.
          // To load the next page, repeat the call with `start` increased by 50.
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

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.tasks.api.scrum.epic.list(
            filter={
                "GROUP_ID": 143,
                ">=ID": 1,
                "<=ID": 50,
                "NAME": "%epic%",
                "!=DESCRIPTION": "old epic",
                "CREATED_BY": 1,
                "MODIFIED_BY": 3,
                "COLOR": "#69dafc",
            },
            order={
                "ID": "asc",
                "NAME": "desc",
            },
            select=[
                "ID",
                "NAME",
                "DESCRIPTION",
                "CREATED_BY",
                "MODIFIED_BY",
                "COLOR",
            ],
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
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.tasks.api.scrum.epic.list(
            filter={
                "GROUP_ID": 143,
                ">=ID": 1,
                "<=ID": 50,
                "NAME": "%epic%",
                "!=DESCRIPTION": "old epic",
                "CREATED_BY": 1,
                "MODIFIED_BY": 3,
                "COLOR": "#69dafc",
            },
            order={
                "ID": "asc",
                "NAME": "desc",
            },
            select=[
                "ID",
                "NAME",
                "DESCRIPTION",
                "CREATED_BY",
                "MODIFIED_BY",
                "COLOR",
            ],
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
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.tasks.api.scrum.epic.list(
            filter={
                "GROUP_ID": 143,
                ">=ID": 1,
                "<=ID": 50,
                "NAME": "%epic%",
                "!=DESCRIPTION": "old epic",
                "CREATED_BY": 1,
                "MODIFIED_BY": 3,
                "COLOR": "#69dafc",
            },
            order={
                "ID": "asc",
                "NAME": "desc",
            },
            select=[
                "ID",
                "NAME",
                "DESCRIPTION",
                "CREATED_BY",
                "MODIFIED_BY",
                "COLOR",
            ],
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "tasks.api.scrum.epic.list", b24.Params{
    	"filter": b24.Params{
    		"GROUP_ID":      143,
    		">=ID":          1,
    		"<=ID":          50,
    		"NAME":          "%epic%",
    		"!=DESCRIPTION": "old epic",
    		"CREATED_BY":    1,
    		"MODIFIED_BY":   3,
    		"COLOR":         "#69dafc",
    	},
    	"order": b24.Params{
    		"ID":   "asc",
    		"NAME": "desc",
    	},
    	"select": []string{"ID", "NAME", "DESCRIPTION", "CREATED_BY", "MODIFIED_BY", "COLOR"},
    	"start":  0,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.api.scrum.epic.list: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "id": 12,
            "groupId": 0,
            "name": "New epic",
            "description": "",
            "createdBy": 1,
            "modifiedBy": 3,
            "color": "#69dafc"
        }
    ],
    "time": {
        "start": 1790263004,
        "finish": 1790263004.141055,
        "duration": 0.14105510711669922,
        "processing": 0,
        "date_start": "2026-09-24T18:16:44+03:00",
        "date_finish": "2026-09-24T18:16:44+03:00",
        "operating_reset_at": 1790263604,
        "operating": 0
    }
}
```

In the example, `groupId` is `0` because the `GROUP_ID` field is not included in the request `select`. If no epics match the conditions, `result` is an empty array. The response contains no `total` or `next` fields.

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../../data-types.md) | Array of epics [(Detailed Description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Epic Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Epic identifier ||
|| **groupId**
[`integer`](../../../data-types.md) | Identifier of the Scrum to which the epic belongs ||
|| **name**
[`string`](../../../data-types.md) | Name of the epic ||
|| **description**
[`string`](../../../data-types.md) | Description of the epic ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the user who created the epic ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | Identifier of the user who last modified the epic. `0` if the epic has not been modified ||
|| **color**
[`string`](../../../data-types.md) | Color of the epic ||
|#

The method does not return attached files. You can retrieve them using the [tasks.api.scrum.epic.get](./tasks-api-scrum-epic-get.md) method.

## Error Handling

The method has no errors of its own. An example of a general error is an application token without the `task` scope:

HTTP Status: **401**

```json
{
    "error": "insufficient_scope",
    "error_description": "The request requires higher privileges than provided by the access token"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-epic-add.md)
- [{#T}](./tasks-api-scrum-epic-update.md)
- [{#T}](./tasks-api-scrum-epic-get.md)
- [{#T}](./tasks-api-scrum-epic-delete.md)
- [{#T}](./tasks-api-scrum-epic-get-fields.md)