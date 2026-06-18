# Get Fields of the Estimate crm.quote.fields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [crm.item.fields](../universal/crm-item-fields.md).

{% endnote %}

The method `crm.quote.fields` returns a description of the fields of the estimate, including custom fields.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.fields  
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"auth":"**put_access_token_here**"}' \
         https://**put_your_bitrix24_address**/rest/crm.quote.fields  
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldDescription = {
      type: string
      isRequired: boolean
      isReadOnly: boolean
      isImmutable: boolean
      isMultiple: boolean
      isDynamic: boolean
      title: string
      statusType?: string
      isDeprecated?: boolean
      listLabel?: string
      formLabel?: string
      filterLabel?: string
      settings?: Record<string, unknown>
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type QuoteFieldsResult = Record<string, FieldDescription>

    try {
      const response = await $b24.actions.v2.call.make<QuoteFieldsResult>({
        method: 'crm.quote.fields',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Quote fields:', Object.keys(result))
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
      async function getQuoteFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.quote.fields',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Quote fields:', Object.keys(result))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getQuoteFields)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.fields',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your data processing logic here
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching quote fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.quote.fields",
        {},
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.dir(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.quote.fields',
        []
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
    "ID": {
      "type": "integer",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "ID"
    },
    "QUOTE_NUMBER": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Estimate Number"
    },
    "TITLE": {
      "type": "string",
      "isRequired": true,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Subject"
    },
    "STATUS_ID": {
      "type": "crm_status",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "statusType": "QUOTE_STATUS",
      "title": "Estimate Stage"
    },
    "CURRENCY_ID": {
      "type": "crm_currency",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Currency"
    },
    "OPPORTUNITY": {
      "type": "double",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Amount"
    },
    "TAX_VALUE": {
      "type": "double",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Tax Rate"
    },
    "COMPANY_ID": {
      "type": "crm_company",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Company"
    },
    "MYCOMPANY_ID": {
      "type": "crm_company",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Your Company Details"
    },
    "CONTACT_ID": {
      "type": "crm_contact",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "isDeprecated": true,
      "title": "Contact"
    },
    "CONTACT_IDS": {
      "type": "crm_contact",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": true,
      "isDynamic": false,
      "title": "CONTACT_IDS"
    },
    "BEGINDATE": {
      "type": "date",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Issue Date"
    },
    "CLOSEDATE": {
      "type": "date",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Deadline"
    },
    "ACTUAL_DATE": {
      "type": "date",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "ACTUAL_DATE"
    },
    "OPENED": {
      "type": "char",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Available to Everyone"
    },
    "CLOSED": {
      "type": "char",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Closed"
    },
    "COMMENTS": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Comment"
    },
    "CONTENT": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Content"
    },
    "TERMS": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Terms"
    },
    "CLIENT_TITLE": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Client"
    },
    "CLIENT_ADDR": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Address"
    },
    "CLIENT_CONTACT": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Contact Person"
    },
    "CLIENT_EMAIL": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "E-mail"
    },
    "CLIENT_PHONE": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Phone"
    },
    "CLIENT_TP_ID": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Tax ID"
    },
    "CLIENT_TPA_ID": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "KPP"
    },
    "ASSIGNED_BY_ID": {
      "type": "user",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Responsible"
    },
    "CREATED_BY_ID": {
      "type": "user",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Created By"
    },
    "MODIFY_BY_ID": {
      "type": "user",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Modified By"
    },
    "DATE_CREATE": {
      "type": "datetime",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Creation Date"
    },
    "DATE_MODIFY": {
      "type": "datetime",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Modification Date"
    },
    "LEAD_ID": {
      "type": "crm_lead",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Lead"
    },
    "DEAL_ID": {
      "type": "crm_deal",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Deal"
    },
    "PERSON_TYPE_ID": {
      "type": "integer",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Client Type"
    },
    "LOCATION_ID": {
      "type": "location",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Location"
    },
    "UTM_SOURCE": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Advertising System"
    },
    "UTM_MEDIUM": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Traffic Type"
    },
    "UTM_CAMPAIGN": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Campaign Identifier"
    },
    "UTM_CONTENT": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Campaign Content"
    },
    "UTM_TERM": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "Campaign Search Condition"
    },
    "LAST_ACTIVITY_TIME": {
      "type": "datetime",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "LAST_ACTIVITY_TIME"
    },
    "LAST_ACTIVITY_BY": {
      "type": "user",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "LAST_ACTIVITY_BY"
    },
    "LAST_COMMUNICATION_TIME": {
      "type": "string",
      "isRequired": false,
      "isReadOnly": true,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": false,
      "title": "LAST_COMMUNICATION_TIME"
    },
    "UF_CRM_6710EFCBAF22C": {
      "type": "date",
      "isRequired": false,
      "isReadOnly": false,
      "isImmutable": false,
      "isMultiple": false,
      "isDynamic": true,
      "title": "UF_CRM_6710EFCBAF22C",
      "listLabel": "End of RK",
      "formLabel": "End of RK",
      "filterLabel": "End of RK",
      "settings": {
        "DEFAULT_VALUE": {
          "TYPE": "NONE",
          "VALUE": ""
        }
      }
    }
  },
  "time": {
    "start": 1750416943.169864,
    "finish": 1750416943.26074,
    "duration": 0.09087610244750977,
    "processing": 0.05633091926574707,
    "date_start": "2025-06-20T13:55:43+02:00",
    "date_finish": "2025-06-20T13:55:43+02:00",
    "operating_reset_at": 1750417543,
    "operating": 0
  }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Object with the description of the fields of the estimate in the format [crm_rest_field_description](../data-types.md#crm_rest_field_description) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Main Fields

#|
|| **Name**
`type` | **Description** ||
|| **ASSIGNED_BY_ID**  
[`integer`](../../data-types.md) | Responsible ||
|| **BEGINDATE**  
[`date`](../../data-types.md) | Date of the estimate issue ||
|| **CLOSED**  
[`char`](../../data-types.md) | Flag for the completion of the estimate ||
|| **CLOSEDATE**  
[`date`](../../data-types.md) | Date of the estimate completion ||
|| **COMMENTS**  
[`string`](../../data-types.md) | Comment ||
|| **COMPANY_ID**  
[`integer`](../../data-types.md) | Identifier of the company associated with the estimate ||
|| **CONTACT_IDS**  
[`integer`](../../data-types.md) | Identifiers of contacts associated with the estimate. Multiple ||
|| **CONTENT**  
[`string`](../../data-types.md) | Content of the estimate ||
|| **CREATED_BY_ID**  
[`integer`](../../data-types.md) | Identifier of the user who created the estimate. Read-only ||
|| **CURRENCY_ID**  
[`crm_currency`](../../data-types.md) | Currency of the estimate ||
|| **DATE_CREATE**  
[`datetime`](../../data-types.md) | Date of the estimate creation. Read-only ||
|| **DATE_MODIFY**  
[`datetime`](../../data-types.md) | Date of the estimate modification. Read-only ||
|| **DEAL_ID**  
[`integer`](../../data-types.md) | Identifier of the deal associated with the estimate ||
|| **ID**  
[`integer`](../../data-types.md) | Identifier of the estimate. Read-only ||
|| **LEAD_ID**  
[`integer`](../../data-types.md) | Identifier of the lead associated with the estimate ||
|| **LOCATION_ID**  
[`integer`](../../data-types.md) | Client's location ||
|| **MODIFY_BY_ID**  
[`integer`](../../data-types.md) | Identifier of the user who modified the estimate. Read-only ||
|| **MYCOMPANY_ID**  
[`integer`](../../data-types.md) | Identifier of the company making the estimate ||
|| **OPENED**  
[`char`](../../data-types.md) | Flag for the availability of the estimate to everyone ||
|| **OPPORTUNITY**  
[`double`](../../data-types.md) | Amount of the estimate ||
|| **PERSON_TYPE_ID**  
[`integer`](../../data-types.md) | Payer type ||
|| **QUOTE_NUMBER**  
[`string`](../../data-types.md) | Estimate number. Read-only ||
|| **STATUS_ID**  
[`crm_status`](../../data-types.md) | Estimate stage. You can get the values of the directory using the method [crm.status.list](../status/crm-status-list.md) with the filter `ENTITY_ID=QUOTE_STATUS` ||
|| **TAX_VALUE**  
[`double`](../../data-types.md) | Tax rate ||
|| **TERMS**  
[`string`](../../data-types.md) | Terms of the estimate ||
|| **TITLE**  
[`string`](../../data-types.md) | Title of the estimate ||
|| **UTM_CAMPAIGN**  
[`string`](../../data-types.md) | Campaign identifier ||
|| **UTM_CONTENT**  
[`string`](../../data-types.md) | Content of the advertising campaign. For example, for contextual ads ||
|| **UTM_MEDIUM**  
[`string`](../../data-types.md) | Type of traffic. For example, CPC for ads or CPM for banners ||
|| **UTM_SOURCE**  
[`string`](../../data-types.md) | Advertising system. For example, Google Ads ||
|| **UTM_TERM**  
[`string`](../../data-types.md) | Campaign search condition. For example, keywords for contextual advertising ||
|| **LAST_ACTIVITY_TIME**  
[`datetime`](../../data-types.md) | Time of the last activity. Read-only ||
|| **LAST_ACTIVITY_BY**  
[`integer`](../../data-types.md) | Identifier of the user who performed the last activity. Read-only ||
|| **LAST_COMMUNICATION_TIME**  
[`string`](../../data-types.md) | Time of the last communication. Read-only ||
|| **UF_...**   | Custom fields. For example, `UF_CRM_25534736`. 

You can add a custom field to the estimate using the method [crm.quote.userfield.add](./user-field/crm-quote-user-field-add.md) ||
|#

#### Deprecated Fields

The following fields are retained only for compatibility and are not recommended for use.

#|
|| **Name**
`type` | **Description** ||
|| **CLIENT_ADDR**  
[`string`](../../data-types.md) | Client's address ||
|| **CLIENT_CONTACT**  
[`string`](../../data-types.md) | Client's contact person ||
|| **CLIENT_EMAIL**  
[`string`](../../data-types.md) | Client's email ||
|| **CLIENT_PHONE**  
[`string`](../../data-types.md) | Client's phone ||
|| **CLIENT_TITLE**  
[`string`](../../data-types.md) | Client's name ||
|| **CLIENT_TP_ID**  
[`string`](../../data-types.md) | Client's Tax ID ||
|| **CLIENT_TPA_ID**  
[`string`](../../data-types.md) | Client's KPP ||
|| **CONTACT_ID**  
[`integer`](../../data-types.md) | Identifier of the contact associated with the estimate ||
|#

## Error Handling

does not return errors.

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-quote-add.md)
- [{#T}](./crm-quote-get.md)
- [{#T}](./crm-quote-update.md)
- [{#T}](./crm-quote-delete.md)
- [{#T}](./user-field/crm-quote-user-field-add.md)