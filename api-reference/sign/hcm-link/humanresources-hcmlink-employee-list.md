# Get Mapped HCM Link Employee List humanresources.hcmlink.employee.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources.hcmlink`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The `humanresources.hcmlink.employee.list` method retrieves a list of mapped employees from the HR system and Bitrix24.

The method can only be executed in the context of authorization of the [application](../../../settings/app-installation/index.md).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **company***
[`string`](../../data-types.md) | Company code in the HR system.

You can get the code using [humanresources.hcmlink.company.list](./humanresources-hcmlink-company-list.md) ||
|| **limit**
[`integer`](../../data-types.md) | Number of records per page.

Allowed values: 1 to 1000. Default value: 100 ||
|| **offset**
[`integer`](../../data-types.md) | Offset for page navigation.

Default value: 0 ||
|| **updatedAt**
[`string`](../../data-types.md) | Modification date in ISO 8601 format. If passed, the method returns records changed after this date ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"company":"hr-company-001","limit":50,"offset":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/humanresources.hcmlink.employee.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make({
        method: 'humanresources.hcmlink.employee.list',
        params: {
          company: 'hr-company-001',
          limit: 50,
          offset: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        console.info(response.getData()!.result)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getHcmLinkEmployees() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'humanresources.hcmlink.employee.list',
            params: {
              company: 'hr-company-001',
              limit: 50,
              offset: 0
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          console.info(response.getData().result)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getHcmLinkEmployees)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.hcmlink.employee.list',
                [
                    'company' => 'hr-company-001',
                    'limit' => 50,
                    'offset' => 0,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting employees: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'humanresources.hcmlink.employee.list',
        {
            company: 'hr-company-001',
            limit: 50,
            offset: 0
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.dir(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'humanresources.hcmlink.employee.list',
        [
            'company' => 'hr-company-001',
            'limit' => 50,
            'offset' => 0,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see section "SDK for Go"
    res, err := client.Core().Call(ctx, "humanresources.hcmlink.employee.list", b24.Params{
    	"company": "hr-company-001",
    	"limit":   50,
    	"offset":  0,
    })
    if err != nil {
    	return fmt.Errorf("humanresources.hcmlink.employee.list: %w", err)
    }

    var items []struct {
    	ID        int `json:"id"`
    	Company   string `json:"company"`
    	Person    string `json:"person"`
    	Employees []struct {
    		ID        int            `json:"id"`
    		Employee  string         `json:"employee"`
    		Data      map[string]any `json:"data"`
    		CreatedAt string         `json:"createdAt"`
    	} `json:"employees"`
    	UserID    int    `json:"userId"`
    	Title     string `json:"title"`
    	CreatedAt string `json:"createdAt"`
    	UpdatedAt string `json:"updatedAt"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("response breakdown: %w", err)
    }
    for _, item := range items {
    	fmt.Println(item.ID, item.Person, item.UserID)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "id": 7,
            "company": "hr-company-001",
            "person": "person-001",
            "employees": [
                {
                    "id": 21,
                    "employee": "employee-001",
                    "data": {
                        "position": "Manager"
                    },
                    "createdAt": "2026-08-06T19:51:02+03:00"
                }
            ],
            "userId": 25,
            "title": "Klaus Weber",
            "createdAt": "2026-08-06T19:51:02+03:00",
            "updatedAt": "2026-08-06T19:51:02+03:00"
        }
    ],
    "time": {
        "start": 1739860000.123,
        "finish": 1739860000.456,
        "duration": 0.333,
        "processing": 0.111,
        "date_start": "2026-08-06T19:51:02+03:00",
        "date_finish": "2026-08-06T19:51:02+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | List of mapped employees [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Request execution time information ||
|#

#### result Array Element {#result}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Individual ID in HCM Link ||
|| **company**
[`string`](../../data-types.md) | Company code in the HR system ||
|| **person**
[`string`](../../data-types.md) | Individual code in the HR system ||
|| **employees**
[`array`](../../data-types.md) | List of HR system employees linked to the individual [(detailed description)](#employees) ||
|| **userId**
[`integer`](../../data-types.md) | Bitrix24 user ID ||
|| **title**
[`string`](../../data-types.md) | Employee name ||
|| **createdAt**
[`string`](../../data-types.md) | Creation date in ISO 8601 format ||
|| **updatedAt**
[`string`](../../data-types.md) | Modification date in ISO 8601 format ||
|#

#### employees Array Element {#employees}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | HCM Link employee ID ||
|| **employee**
[`string`](../../data-types.md) | Employee code in the HR system ||
|| **data**
[`object`](../../data-types.md) | Employee data ||
|| **createdAt**
[`string`](../../data-types.md) | Creation date in ISO 8601 format ||
|#

## Error Handling

HTTP status: **200**, **403**

```json
{
    "error": 510,
    "error_description": "Operation failed"
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **When It Occurs** ||
|| `510` | Operation failed | The company was not found or the `company` parameter is not passed ||
|| `ACCESS_DENIED` | Access denied! Access denied. | The user is not an administrator ||
|| `WRONG_AUTH_TYPE` | Application context required | The method was not called in the application context ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-hcmlink-employee-set.md)
- [{#T}](./humanresources-hcmlink-field-value-set.md)
