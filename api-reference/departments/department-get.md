# Get the list of departments department.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`department`](../scopes/permissions.md)
>
> Who can execute the method: any user

The `department.get` method returns a list of active departments in the company structure, for example, all departments within one parent department. Write the filter names `ID`, `NAME`, `SORT`, `PARENT`, and `UF_HEAD` in uppercase: the method ignores a filter in any other case.

The method belongs to the previous API version. The REST 3.0 counterpart is [humanresources.node.list](./node/humanresources-node-list.md); method correspondence is described in the [section overview](./index.md#v2-v3).

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **sort**
[`string`](../data-types.md) | The name of the field to sort by. Sorting works on the following fields:
- `ID` — department identifier
- `NAME` — department name
- `SORT` — sorting order
- `PARENT` — parent department identifier
- `UF_HEAD` — department head user identifier ||
|| **order**
[`string`](../data-types.md) | Sorting direction, `ASC` by default:
- `ASC` — ascending
- `DESC` — descending ||
|| **ID**
[`integer`](../data-types.md) | Filter by department identifier ||
|| **NAME**
[`string`](../data-types.md) | Filter by the full department name ||
|| **SORT**
[`integer`](../data-types.md) | Filter by sorting field. The method accepts the parameter but does not apply it: the response includes departments with any `SORT` value ||
|| **PARENT**
[`integer`](../data-types.md) | Filter by parent department identifier. To retrieve the top-level department, pass `0` ||
|| **UF_HEAD**
[`integer`](../data-types.md) | Filter by department head identifier ||
|| **start**
[`integer`](../data-types.md) | This parameter is used for pagination control.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number.

Details in the article [{#T}](../../settings/how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "sort": "NAME",
        "order": "DESC",
        "PARENT": 1
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/department.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "sort": "NAME",
        "order": "DESC",
        "PARENT": 1,
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/department.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type Department = {
      ID: string
      NAME: string
      SORT: number
      PARENT?: string
      UF_HEAD?: string
    }

    try {
      const response = await $b24.actions.v2.call.make<Department[]>({
        method: 'department.get',
        params: {
          sort: 'NAME',
          order: 'DESC',
          PARENT: 1,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.map((department) => department.NAME))
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
      async function getDepartments() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'department.get',
            params: {
              sort: 'NAME',
              order: 'DESC',
              PARENT: 1,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.map((department) => department.NAME))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getDepartments)
    </script>
    ```

- Python

    Example

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.department.get(
            sort="NAME",
            order="DESC",
            parent=1,
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
        bitrix_response = client.department.get(
            sort="NAME",
            order="DESC",
            parent=1,
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

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'department.get',
                [
                    'sort'   => 'NAME',
                    'order'  => 'DESC',
                    'PARENT' => 1,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'department.get',
        {
            sort: 'NAME',
            order: 'DESC',
            PARENT: 1,
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'department.get',
        [
            'sort'   => 'NAME',
            'order'  => 'DESC',
            'PARENT' => 1,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "department.get", b24.Params{
    	"sort":   "NAME",
    	"order":  "DESC",
    	"PARENT": 1,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("department.get: %w", err)
    }

    var items []struct {
    	ID     b24.ID `json:"ID"`
    	Name   string `json:"NAME"`
    	Sort   int    `json:"SORT"`
    	Parent string `json:"PARENT"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.Name)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
  "result": [
    {
      "ID": "3",
      "NAME": "Sales Department",
      "SORT": 500,
      "PARENT": "1"
    },
    {
      "ID": "15",
      "NAME": "R&D",
      "SORT": 500,
      "PARENT": "1"
    },
    {
      "ID": "4",
      "NAME": "Marketing and Advertising Department",
      "SORT": 500,
      "PARENT": "1"
    },
    {
      "ID": "17",
      "NAME": "Legal Department",
      "SORT": 500,
      "PARENT": "1"
    },
    {
      "ID": "2",
      "NAME": "Accounting",
      "SORT": 500,
      "PARENT": "1"
    }
  ],
  "total": 5,
  "time": {
    "start": 1736924749.531017,
    "finish": 1736924749.719845,
    "duration": 0.1888279914855957,
    "processing": 0.00179290771484375,
    "date_start": "2025-01-15T07:05:49+00:00",
    "date_finish": "2025-01-15T07:05:49+00:00",
    "operating": 0
  }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`department[]`](#st_department) | List of departments that match the filter ||
|| **next**
[`integer`](../data-types.md) | The `start` value for the next page. The field is absent on the last page ||
|| **total**
[`integer`](../data-types.md) | The total number of records found ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

### Structure department {#st_department}

```json
{
    "ID": "2",
    "NAME": "Accounting",
    "SORT": 500,
    "PARENT": "1",
    "UF_HEAD": "1"
}
```

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../data-types.md) | Department identifier. The number is returned as a string ||
|| **NAME**
[`string`](../data-types.md) | Department name ||
|| **SORT**
[`integer`](../data-types.md) | Department sorting field ||
|| **PARENT**
[`string`](../data-types.md) | Parent department identifier. The number is returned as a string. The top-level department does not have this field ||
|| **UF_HEAD**
[`string`](../data-types.md) | Identifier of the department head user. The number is returned as a string. If no head is assigned, the field is absent or equals `"0"` ||
|#

## Error Handling

The method has no errors of its own. The method ignores an unknown field in `sort`. If no departments match the filter, `result` is an empty array.

{% include notitle [error handling](../../_includes/error-info.md) %}

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./department-add.md)
- [{#T}](./department-update.md)
- [{#T}](./department-delete.md)
- [{#T}](./department-fields.md)