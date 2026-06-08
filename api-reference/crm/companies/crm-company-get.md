# Get Company Information crm.company.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: user with "Read" access permission for companies

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [crm.item.get](../universal/crm-item-get.md).

{% endnote %}

The method `crm.company.get` returns a company by its identifier.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Company identifier ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":12}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":12,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.company.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CrmCompanyResult = {
      ID: string
      TITLE: string
      COMPANY_TYPE: string
      ASSIGNED_BY_ID: string
      OPENED: string
      CURRENCY_ID: string
      DATE_CREATE: ISODate | null
      DATE_MODIFY: ISODate | null
      LAST_ACTIVITY_TIME: ISODate | null
    }

    try {
      const response = await $b24.actions.v2.call.make<CrmCompanyResult>({
        method: 'crm.company.get',
        params: {
          id: 12,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Company:', result.ID, result.TITLE)
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
      async function getCompany() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.company.get',
            params: {
              id: 12,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Company:', result.ID, result.TITLE)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCompany)
    </script>
    ```

- PHP

    ```php
    $id = readline("Enter ID: ");
    
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.company.get',
                [
                    'id' => $id,
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
        echo 'Error getting company: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.company.get",
        { id: id },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.company.get',
        [
            'ID' => 12
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "ID": "2909",
        "COMPANY_TYPE": "CUSTOMER",
        "TITLE": "GAZPROM",
        "LOGO": null,
        "LEAD_ID": null,
        "HAS_PHONE": "N",
        "HAS_EMAIL": "N",
        "HAS_IMOL": "N",
        "ASSIGNED_BY_ID": "139",
        "CREATED_BY_ID": "835",
        "MODIFY_BY_ID": "1",
        "BANKING_DETAILS": "7736050003",
        "INDUSTRY": "IT",
        "REVENUE": "0",
        "CURRENCY_ID": "EUR",
        "EMPLOYEES": "EMPLOYEES_1",
        "COMMENTS": "",
        "DATE_CREATE": "2024-03-11T16:36:46+01:00",
        "DATE_MODIFY": "2024-08-01T16:08:08+01:00",
        "OPENED": "N",
        "IS_MY_COMPANY": "N",
        "ORIGINATOR_ID": null,
        "ORIGIN_ID": null,
        "ORIGIN_VERSION": null,
        "ADDRESS": null,
        "ADDRESS_2": null,
        "ADDRESS_CITY": null,
        "ADDRESS_POSTAL_CODE": null,
        "ADDRESS_REGION": null,
        "ADDRESS_PROVINCE": null,
        "ADDRESS_COUNTRY": null,
        "ADDRESS_COUNTRY_CODE": null,
        "ADDRESS_LOC_ADDR_ID": null,
        "ADDRESS_LEGAL": null,
        "REG_ADDRESS": null,
        "REG_ADDRESS_2": null,
        "REG_ADDRESS_CITY": null,
        "REG_ADDRESS_POSTAL_CODE": null,
        "REG_ADDRESS_REGION": null,
        "REG_ADDRESS_PROVINCE": null,
        "REG_ADDRESS_COUNTRY": null,
        "REG_ADDRESS_COUNTRY_CODE": null,
        "REG_ADDRESS_LOC_ADDR_ID": null,
        "UTM_SOURCE": null,
        "UTM_MEDIUM": null,
        "UTM_CAMPAIGN": null,
        "UTM_CONTENT": null,
        "UTM_TERM": null,
        "PARENT_ID_156": null,
        "LAST_ACTIVITY_BY": "835",
        "LAST_ACTIVITY_TIME": "2024-03-11T16:36:46+01:00",
        "LAST_COMMUNICATION_TIME": null,
        "UF_CRM_1687188367": ""
    },
    "time": {
        "start": 1769497419,
        "finish": 1769497419.915092,
        "duration": 0.9150919914245605,
        "processing": 0,
        "date_start": "2026-01-27T10:03:39+01:00",
        "date_finish": "2026-01-27T10:03:39+01:00",
        "operating_reset_at": 1769498019,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response, returns company fields. You can get the description of company fields using the method [crm.company.fields](./crm-company-fields.md) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | `Access denied` | The user does not have "Read" access permission for companies ||
|| `-` | `Not found` | Company not found ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-company-add.md)
- [{#T}](./crm-company-list.md)
- [{#T}](./crm-company-update.md)
- [{#T}](./crm-company-delete.md)
- [{#T}](./crm-company-fields.md)