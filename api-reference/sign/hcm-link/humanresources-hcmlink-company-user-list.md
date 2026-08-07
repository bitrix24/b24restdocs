# Get Current User HCM Link Company List humanresources.hcmlink.company.user.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources.hcmlink`](../../scopes/permissions.md)
>
> Who can execute the method: authorized user

The `humanresources.hcmlink.company.user.list` method retrieves companies from an HR system linked to the current Bitrix24 user.

The method can only be executed in the context of authorization of the [application](../../../settings/app-installation/index.md).

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | Number of records per page.

Allowed values: 1 to 1000. Default value: 100 ||
|| **offset**
[`integer`](../../data-types.md) | Offset for page navigation.

Default value: 0 ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"limit":50,"offset":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/humanresources.hcmlink.company.user.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make({
        method: 'humanresources.hcmlink.company.user.list',
        params: {
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
      async function getUserHcmLinkCompanies() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'humanresources.hcmlink.company.user.list',
            params: {
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

      document.addEventListener('DOMContentLoaded', getUserHcmLinkCompanies)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.hcmlink.company.user.list',
                [
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
        echo 'Error getting user companies: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'humanresources.hcmlink.company.user.list',
        {
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
        'humanresources.hcmlink.company.user.list',
        [
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
    res, err := client.Core().Call(ctx, "humanresources.hcmlink.company.user.list", b24.Params{
    	"limit":  50,
    	"offset": 0,
    })
    if err != nil {
    	return fmt.Errorf("humanresources.hcmlink.company.user.list: %w", err)
    }

    var items []struct {
    	ID           int            `json:"id"`
    	Company      string         `json:"company"`
    	CrmCompanyID int            `json:"crmCompanyId"`
    	Title        string         `json:"title"`
    	Data         map[string]any `json:"data"`
    	Person       string         `json:"person"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("response breakdown: %w", err)
    }
    for _, item := range items {
    	fmt.Println(item.ID, item.Company, item.Person)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "id": 15,
            "company": "hr-company-001",
            "crmCompanyId": 12,
            "title": "Muller GmbH",
            "data": {
                "taxId": "1234567890"
            },
            "person": "person-001"
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
[`array`](../../data-types.md) | List of HCM Link companies linked to the current user [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Request execution time information ||
|#

#### result Array Element {#result}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | HCM Link company ID ||
|| **company**
[`string`](../../data-types.md) | Company code in the HR system ||
|| **crmCompanyId**
[`integer`](../../data-types.md) | CRM company ID ||
|| **title**
[`string`](../../data-types.md) | Company name ||
|| **data**
[`object`](../../data-types.md) | Additional company data ||
|| **person**
[`string`](../../data-types.md) | Individual code in the HR system ||
|#

## Error Handling

HTTP status: **200**, **403**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Application context required"
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **When It Occurs** ||
|| `ACCESS_DENIED` | User authorization required. | The user is not authorized ||
|| `WRONG_AUTH_TYPE` | Application context required | The method was not called in the application context ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-hcmlink-company-list.md)
- [{#T}](./humanresources-hcmlink-employee-list.md)
