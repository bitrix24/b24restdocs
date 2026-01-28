# Get Company Field Descriptions crm.company.fields

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "Method Development Stopped" %}

The method `crm.company.fields` continues to function, but there is a more relevant alternative [crm.item.fields](../universal/crm-item-fields.md).

{% endnote %}

The method `crm.company.fields` returns descriptions of company fields, including [custom fields](./userfields/index.md).

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
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.company.fields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.company.fields',
    		{}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.dir(result);
    	}
    }
    catch(error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.company.fields',
                []
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
        echo 'Error fetching company fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.company.fields",
        {},
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
        'crm.company.fields',
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
        "TITLE": {
            "type": "string",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Company Name"
        },
        "COMPANY_TYPE": {
            "type": "crm_status",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "statusType": "COMPANY_TYPE",
            "title": "Company Type"
        },
        "LOGO": {
            "type": "file",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Logo"
        },
        "ADDRESS": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Physical Address"
        },
        "ADDRESS_2": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Address (line 2)"
        },
        "ADDRESS_CITY": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "City"
        },
        "ADDRESS_POSTAL_CODE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Postal Code"
        },
        "ADDRESS_REGION": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Region"
        },
        "ADDRESS_PROVINCE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Province"
        },
        "ADDRESS_COUNTRY": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Country"
        },
        "ADDRESS_COUNTRY_CODE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Country Code"
        },
        "ADDRESS_LOC_ADDR_ID": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Location Address ID"
        },
        "ADDRESS_LEGAL": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Legal Address"
        },
        "REG_ADDRESS": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Legal Address"
        },
        "REG_ADDRESS_2": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Legal Address (line 2)"
        },
        "REG_ADDRESS_CITY": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Legal Address City"
        },
        "REG_ADDRESS_POSTAL_CODE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Legal Address Postal Code"
        },
        "REG_ADDRESS_REGION": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Legal Address Region"
        },
        "REG_ADDRESS_PROVINCE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Legal Address Province"
        },
        "REG_ADDRESS_COUNTRY": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Legal Address Country"
        },
        "REG_ADDRESS_COUNTRY_CODE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Legal Address Country Code"
        },
        "REG_ADDRESS_LOC_ADDR_ID": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Legal Address Location ID"
        },
        "BANKING_DETAILS": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Banking Details"
        },
        "INDUSTRY": {
            "type": "crm_status",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "statusType": "INDUSTRY",
            "title": "Industry"
        },
        "EMPLOYEES": {
            "type": "crm_status",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "statusType": "EMPLOYEES",
            "title": "Number of Employees"
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
        "REVENUE": {
            "type": "double",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Annual Revenue"
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
        "COMMENTS": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Comment"
        },
        "HAS_PHONE": {
            "type": "char",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Phone Set"
        },
        "HAS_EMAIL": {
            "type": "char",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "E-mail Set"
        },
        "HAS_IMOL": {
            "type": "char",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Open Line Set"
        },
        "IS_MY_COMPANY": {
            "type": "char",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "My Company"
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
        "CONTACT_ID": {
            "type": "crm_contact",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": true,
            "isDynamic": false,
            "title": "Contact"
        },
        "LEAD_ID": {
            "type": "crm_lead",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Lead",
            "settings": {
                "parentEntityTypeId": 1
            }
        },
        "ORIGINATOR_ID": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "External Source"
        },
        "ORIGIN_ID": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Element ID in External Source"
        },
        "ORIGIN_VERSION": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Original Version"
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
            "title": "Campaign Designation"
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
        "PARENT_ID_156": {
            "type": "crm_entity",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Purchase",
            "settings": {
                "parentEntityTypeId": 156
            }
        },
        "LAST_ACTIVITY_TIME": {
            "type": "datetime",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Last Activity"
        },
        "LAST_ACTIVITY_BY": {
            "type": "user",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Last Activity Author in Timeline"
        },
        "LAST_COMMUNICATION_TIME": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Last Communication Date"
        },
        "PHONE": {
            "type": "crm_multifield",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": true,
            "isDynamic": false,
            "title": "Phone"
        },
        "EMAIL": {
            "type": "crm_multifield",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": true,
            "isDynamic": false,
            "title": "E-mail"
        },
        "WEB": {
            "type": "crm_multifield",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": true,
            "isDynamic": false,
            "title": "Website"
        },
        "IM": {
            "type": "crm_multifield",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": true,
            "isDynamic": false,
            "title": "Messenger"
        },
        "LINK": {
            "type": "crm_multifield",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": true,
            "isDynamic": false,
            "title": "LINK"
        },
        "UF_CRM_1687188367": {
            "type": "crm",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": true,
            "title": "UF_CRM_1687188367",
            "listLabel": "Subsidiary",
            "formLabel": "Subsidiary",
            "filterLabel": "Subsidiary",
            "settings": {
                "COMPANY": "Y",
                "LEAD": null
            }
        },
        "UF_CRM_64A29A1CD5CC2": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": true,
            "title": "UF_CRM_64A29A1CD5CC2",
            "listLabel": "New Field",
            "formLabel": "New Field",
            "filterLabel": "New Field",
            "settings": {
                "SIZE": 20,
                "ROWS": 1,
                "REGEXP": "",
                "MIN_LENGTH": 0,
                "MAX_LENGTH": 0,
                "DEFAULT_VALUE": ""
            }
        }
    },
    "time": {
        "start": 1769494687,
        "finish": 1769494687.641697,
        "duration": 0.6416969299316406,
        "processing": 0,
        "date_start": "2026-01-27T09:18:07+01:00",
        "date_finish": "2026-01-27T09:18:07+01:00",
        "operating_reset_at": 1769495287,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Company Identifier ||
|| **TITLE**
[`string`](../../data-types.md) | Company Name ||
|| **COMPANY_TYPE**
[`crm_status`](../../data-types.md) | Company Type. Values can be obtained using the method [crm.status.list](../status/crm-status-list.md) with a filter by `ENTITY_ID=COMPANY_TYPE` ||
|| **LOGO**
[`file`](../../data-types.md) | Logo ||
|| **ADDRESS**
[`string`](../../data-types.md) | Physical Address. Deprecated, used for compatibility. For working with the address, use [requisites](../requisites/index.md).

Deprecated address fields:
- `ADDRESS_2` — address (line 2)
- `ADDRESS_CITY` — city
- `ADDRESS_POSTAL_CODE` — postal code
- `ADDRESS_REGION` — region
- `ADDRESS_PROVINCE` — province
- `ADDRESS_COUNTRY` — country
- `ADDRESS_COUNTRY_CODE` — country code
- `ADDRESS_LOC_ADDR_ID` — location address ID
- `ADDRESS_LEGAL` — legal address
- `REG_ADDRESS` — legal address
- `REG_ADDRESS_2` — legal address (line 2)
- `REG_ADDRESS_CITY` — legal address city
- `REG_ADDRESS_POSTAL_CODE` — legal address postal code
- `REG_ADDRESS_REGION` — legal address region
- `REG_ADDRESS_PROVINCE` — legal address province
- `REG_ADDRESS_COUNTRY` — legal address country
- `REG_ADDRESS_COUNTRY_CODE` — legal address country code
- `REG_ADDRESS_LOC_ADDR_ID` — legal address location ID ||
|| **BANKING_DETAILS**
[`string`](../../data-types.md) | Banking Details ||
|| **INDUSTRY**
[`crm_status`](../../data-types.md) | Industry. Values can be obtained using the method [crm.status.list](../status/crm-status-list.md) with a filter by `ENTITY_ID=INDUSTRY` ||
|| **EMPLOYEES**
[`crm_status`](../../data-types.md) | Number of Employees. Values can be obtained using the method [crm.status.list](../status/crm-status-list.md) with a filter by `ENTITY_ID=EMPLOYEES` ||
|| **CURRENCY_ID**
[`crm_currency`](../../data-types.md) | Currency ||
|| **REVENUE**
[`double`](../../data-types.md) | Annual Revenue ||
|| **OPENED**
[`char`](../../data-types.md) | Is the company available to everyone. Possible values: `Y` or `N` ||
|| **COMMENTS**
[`string`](../../data-types.md) | Comment ||
|| **HAS_PHONE**
[`char`](../../data-types.md) | Phone Set ||
|| **HAS_EMAIL**
[`char`](../../data-types.md) | E-mail Set ||
|| **HAS_IMOL**
[`char`](../../data-types.md) | Open Line Set ||
|| **IS_MY_COMPANY**
[`char`](../../data-types.md) | My Company Indicator. Possible values: `Y` or `N` ||
|| **ASSIGNED_BY_ID**
[`user`](../../data-types.md) | Responsible ||
|| **CREATED_BY_ID**
[`user`](../../data-types.md) | Created By ||
|| **MODIFY_BY_ID**
[`user`](../../data-types.md) | Modified By ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Creation Date ||
|| **DATE_MODIFY**
[`datetime`](../../data-types.md) | Modification Date ||
|| **CONTACT_ID**
[`crm_contact`](../../data-types.md) | Contact. Multiple ||
|| **LEAD_ID**
[`crm_lead`](../../data-types.md) | Lead ID associated with the company ||
|| **ORIGINATOR_ID**
[`string`](../../data-types.md) | Data Source Identifier. Used only for linking to an external source ||
|| **ORIGIN_ID**
[`string`](../../data-types.md) | Element ID in the data source. Used only for linking to an external source ||
|| **ORIGIN_VERSION**
[`string`](../../data-types.md) | Original Version. Used to protect data from accidental overwriting by an external system ||
|| **UTM_SOURCE**
[`string`](../../data-types.md) | Advertising System. Google Ads, Facebook Ads, etc. ||
|| **UTM_MEDIUM**
[`string`](../../data-types.md) | Traffic Type. Possible values:
- `CPC` — ads
- `CPM` — banners ||
|| **UTM_CAMPAIGN**
[`string`](../../data-types.md) | Campaign Designation ||
|| **UTM_CONTENT**
[`string`](../../data-types.md) | Campaign Content. For example, for contextual ads ||
|| **UTM_TERM**
[`string`](../../data-types.md) | Campaign Search Condition. For example, keywords for contextual advertising ||
||**PARENT_ID_...** | Relationship Fields.

If there are SPAs related to companies in the account, there is a field for each such SPA that stores the relationship between that SPA and the company. The field itself stores the identifier of the element of that SPA ||
|| **LAST_ACTIVITY_TIME**
[`datetime`](../../data-types.md) | Last Activity ||
|| **LAST_ACTIVITY_BY**
[`user`](../../data-types.md) | Author of the last activity in the timeline ||
|| **LAST_COMMUNICATION_TIME**
[`string`](../../data-types.md) | Last Communication Date ||
|| **PHONE**
[`crm_multifield`](../../data-types.md) | Phone. Multiple ||
|| **EMAIL**
[`crm_multifield`](../../data-types.md) | E-mail. Multiple ||
|| **WEB**
[`crm_multifield`](../../data-types.md) | Website. Multiple ||
|| **IM**
[`crm_multifield`](../../data-types.md) | Messenger. Multiple ||
|| **LINK**
[`crm_multifield`](../../data-types.md) | LINK. Multiple ||
||**UF_...**  | Custom Fields. For example, `UF_CRM_25534736`.

Depending on the account settings, companies may have a set of custom fields of specific types.

You can add a custom field to a company using the method [crm.company.userfield.add](./userfields/crm-company-userfield-add.md) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | `Access denied` | The user does not have permission for "Read" companies ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-company-add.md)
- [{#T}](./crm-company-get.md)
- [{#T}](./crm-company-list.md)
- [{#T}](./crm-company-update.md)
- [{#T}](./crm-company-delete.md)