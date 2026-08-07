# Add HCM Link Company humanresources.hcmlink.company.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources.hcmlink`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The `humanresources.hcmlink.company.add` method adds a company from an HR system to the HCM Link integration.

The method can only be executed in the context of authorization of the [application](../../../settings/app-installation/index.md).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Company data from the HR system [(detailed description)](#fields) ||
|#

### fields Parameter {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **company***
[`string`](../../data-types.md) | Company code in the HR system ||
|| **crmCompanyId***
[`integer`](../../data-types.md) | CRM company ID.

You can get the ID using [crm.item.list](../../crm/universal/crm-item-list.md) with the `entityTypeId = 4` parameter and the `isMyCompany = Y` filter ||
|| **title***
[`string`](../../data-types.md) | Company name ||
|| **data**
[`object`](../../data-types.md) | Additional company data ||
|| **fields***
[`array`](../../data-types.md) | List of HR system fields [(detailed description)](#field) ||
|#

### fields.fields Array Element {#field}

#|
|| **Name**
`type` | **Description** ||
|| **field***
[`string`](../../data-types.md) | Field code in the HR system ||
|| **title***
[`string`](../../data-types.md) | Field name ||
|| **type**
[`string`](../../data-types.md) | Field type. If no value is passed, the method saves the field with the internal `UNKNOWN` type.

Possible values:

- `STRING` — string
- `FIRST_NAME` — first name
- `LAST_NAME` — last name
- `PATRONYMIC_NAME` — middle name
- `PHONE` — phone
- `EMAIL` — email address
- `ADDRESS` — address
- `BIRTHDAY` — date of birth
- `POSITION` — position
- `DEPARTMENT` — department
- `DOCUMENT_REGISTRATION_NUMBER` — document registration number
- `DOCUMENT_UID` — unique document ID
- `DOCUMENT_DATE` — document date ||
|| **entityType**
[`string`](../../data-types.md) | Type of object the field belongs to. If no value is passed, the method saves the field with the internal `UNKNOWN` type.

Possible values:

- `EMPLOYEE` — employee
- `COMPANY` — company
- `DOCUMENT` — document ||
|| **ttl**
[`integer`](../../data-types.md) | Field value storage time in seconds.

Default value: 86400 ||
|#

## Code Examples

{% include [Examples note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"company":"hr-company-001","crmCompanyId":12,"title":"Muller GmbH","data":{"taxId":"1234567890"},"fields":[{"field":"personal_number","title":"Personnel Number","type":"STRING","entityType":"EMPLOYEE","ttl":86400}]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/humanresources.hcmlink.company.add
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make({
        method: 'humanresources.hcmlink.company.add',
        params: {
          fields: {
            company: 'hr-company-001',
            crmCompanyId: 12,
            title: 'Muller GmbH',
            data: {
              taxId: '1234567890',
            },
            fields: [
              {
                field: 'personal_number',
                title: 'Personnel Number',
                type: 'STRING',
                entityType: 'EMPLOYEE',
                ttl: 86400,
              },
            ],
          },
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
      async function addHcmLinkCompany() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'humanresources.hcmlink.company.add',
            params: {
              fields: {
                company: 'hr-company-001',
                crmCompanyId: 12,
                title: 'Muller GmbH',
                data: {
                  taxId: '1234567890'
                },
                fields: [
                  {
                    field: 'personal_number',
                    title: 'Personnel Number',
                    type: 'STRING',
                    entityType: 'EMPLOYEE',
                    ttl: 86400
                  }
                ]
              }
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

      document.addEventListener('DOMContentLoaded', addHcmLinkCompany)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.hcmlink.company.add',
                [
                    'fields' => [
                        'company' => 'hr-company-001',
                        'crmCompanyId' => 12,
                        'title' => 'Muller GmbH',
                        'data' => [
                            'taxId' => '1234567890',
                        ],
                        'fields' => [
                            [
                                'field' => 'personal_number',
                                'title' => 'Personnel Number',
                                'type' => 'STRING',
                                'entityType' => 'EMPLOYEE',
                                'ttl' => 86400,
                            ],
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding company: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'humanresources.hcmlink.company.add',
        {
            fields: {
                company: 'hr-company-001',
                crmCompanyId: 12,
                title: 'Muller GmbH',
                data: { taxId: '1234567890' },
                fields: [{ field: 'personal_number', title: 'Personnel Number', type: 'STRING', entityType: 'EMPLOYEE', ttl: 86400 }]
            }
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
        'humanresources.hcmlink.company.add',
        [
            'fields' => [
                'company' => 'hr-company-001',
                'crmCompanyId' => 12,
                'title' => 'Muller GmbH',
                'data' => [
                    'taxId' => '1234567890',
                ],
                'fields' => [
                    [
                        'field' => 'personal_number',
                        'title' => 'Personnel Number',
                        'type' => 'STRING',
                        'entityType' => 'EMPLOYEE',
                        'ttl' => 86400,
                    ],
                ],
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the "SDK for Go" section
    res, err := client.Core().Call(ctx, "humanresources.hcmlink.company.add", b24.Params{
    	"fields": b24.Params{
    		"company":      "hr-company-001",
    		"crmCompanyId": 12,
    		"title":        "Muller GmbH",
    		"data": b24.Params{
    			"taxId": "1234567890",
    		},
    		"fields": []b24.Params{
    			{
    				"field":      "personal_number",
    				"title":      "Personnel Number",
    				"type":       "STRING",
    				"entityType": "EMPLOYEE",
    				"ttl":        86400,
    			},
    		},
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("humanresources.hcmlink.company.add: %w", err)
    }

    var id int
    if err := json.Unmarshal(res.Result, &id); err != nil {
    	return fmt.Errorf("response parsing: %w", err)
    }
    fmt.Println(id)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 15,
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
[`integer`](../../data-types.md) | ID of the added HCM Link company ||
|| **time**
[`time`](../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **200**, **403**

```json
{
    "error": 0,
    "error_description": "Operation failed"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **When It Occurs** ||
|| `0` | Operation failed | `crmCompanyId`, `company`, `title`, or `fields` is not passed; a save error occurred ||
|| `510` | Operation failed | CRM company was not found ||
|| `ACCESS_DENIED` | Access denied! Access denied. | The user is not an administrator ||
|| `WRONG_AUTH_TYPE` | Application context required | The method was not called in the application context ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-hcmlink-company-update.md)
- [{#T}](./humanresources-hcmlink-company-list.md)
- [{#T}](./humanresources-hcmlink-employee-set.md)
