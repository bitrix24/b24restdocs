# Get Fields of the Estimate crm.quote.fields

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.quote.fields` returns the description of the fields of the estimate, including custom fields.

## Method Parameters

No parameters.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

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

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.quote.fields",
    		{}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
    }
    catch( error )
    {
    	console.error(error);
    }
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
        // Your logic for processing data
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

HTTP status: **200**

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
[`date`](../../data-types.md) | Issue date of the estimate ||
|| **CLOSED**  
[`char`](../../data-types.md) | Flag for completion of the estimate ||
|| **CLOSEDATE**  
[`date`](../../data-types.md) | Completion date of the estimate ||
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
[`datetime`](../../data-types.md) | Creation date of the estimate. Read-only ||
|| **DATE_MODIFY**  
[`datetime`](../../data-types.md) | Modification date of the estimate. Read-only ||
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
[`char`](../../data-types.md) | Flag for availability of the estimate to everyone ||
|| **OPPORTUNITY**  
[`double`](../../data-types.md) | Amount of the estimate ||
|| **PERSON_TYPE_ID**  
[`integer`](../../data-types.md) | Payer type ||
|| **QUOTE_NUMBER**  
[`string`](../../data-types.md) | Estimate number. Read-only ||
|| **STATUS_ID**  
[`crm_status`](../../data-types.md) | Stage of the estimate. You can get the values of the directory using the method [crm.status.list](../status/crm-status-list.md) with the filter `ENTITY_ID=QUOTE_STATUS` ||
|| **TAX_VALUE**  
[`double`](../../data-types.md) | Tax rate ||
|| **TERMS**  
[`string`](../../data-types.md) | Terms of the estimate ||
|| **TITLE**  
[`string`](../../data-types.md) | Title of the estimate ||
|| **UTM_CAMPAIGN**  
[`string`](../../data-types.md) | dvertising campaign identifier ||
|| **UTM_CONTENT**  
[`string`](../../data-types.md) | Content of the advertising campaign. For example, for contextual ads ||
|| **UTM_MEDIUM**  
[`string`](../../data-types.md) | Traffic type. For example, CPC for ads or CPM for banners ||
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

The following fields are kept only for compatibility and are not recommended for use.

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

The method does not return errors.

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-quote-add.md)
- [{#T}](./crm-quote-get.md)
- [{#T}](./crm-quote-update.md)
- [{#T}](./crm-quote-delete.md)
- [{#T}](./user-field/crm-quote-user-field-add.md)