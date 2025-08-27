# Get Contact Fields crm.contact.fields

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns the description of contact fields, including custom ones.

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
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.fields
    ```
  

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.fields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.contact.fields',
    		{}
    	);
    	
    	const result = response.getData().result;
    	console.info('Contact fields', result);
    }
    catch( error )
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
                'crm.contact.fields',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Contact fields: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching contact fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.contact.fields",
        {},
        (result) => {
            if(result.error())
                console.error(result.error());
            else
                console.info("Contact fields", result.data());
        }
    );    
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.contact.fields',
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
        "HONORIFIC": {
        "type": "crm_status",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "statusType": "HONORIFIC",
        "title": "Salutation"
        },
        "NAME": {
        "type": "string",
        "isRequired": true,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "title": "First Name"
        },
        "SECOND_NAME": {
        "type": "string",
        "isRequired": true,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "title": "Middle Name"
        },
        "LAST_NAME": {
        "type": "string",
        "isRequired": true,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "title": "Last Name"
        },
        "PHOTO": {
        "type": "file",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "title": "Photo"
        },
        "BIRTHDATE": {
        "type": "date",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "title": "Birthdate"
        },
        "TYPE_ID": {
        "type": "crm_status",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "statusType": "CONTACT_TYPE",
        "title": "Contact Type"
        },
        "SOURCE_ID": {
        "type": "crm_status",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "statusType": "SOURCE",
        "title": "Source"
        },
        "SOURCE_DESCRIPTION": {
        "type": "string",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "title": "Additional Source Information"
        },
        "POST": {
        "type": "string",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "title": "Position"
        },
        "ADDRESS": {
        "type": "string",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "title": "Address"
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
        "COMMENTS": {
        "type": "string",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "title": "Comment"
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
        "EXPORT": {
        "type": "char",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "title": "Participates in Contact Export"
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
        "title": "Open Channel Set"
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
        "COMPANY_ID": {
        "type": "crm_company",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "isDeprecated": true,
        "title": "Company"
        },
        "COMPANY_IDS": {
        "type": "crm_company",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": true,
        "isDynamic": false,
        "title": "COMPANY_IDS"
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
        "title": "Identifier in External Source"
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
        "FACE_ID": {
        "type": "integer",
        "isRequired": false,
        "isReadOnly": false,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "title": "Link to Faces from the FaceID Module"
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
        "title": "Last Activity"
        },
        "LAST_ACTIVITY_BY": {
        "type": "user",
        "isRequired": false,
        "isReadOnly": true,
        "isImmutable": false,
        "isMultiple": false,
        "isDynamic": false,
        "title": "Last Activity Author"
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
        }
    },
    "time": {
        "start": 1715004755.782705,
        "finish": 1715004756.118899,
        "duration": 0.3361940383911133,
        "processing": 0.10344505310058594,
        "date_start": "2024-05-06T17:12:35+02:00",
        "date_finish": "2024-05-06T17:12:36+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
||**ID**
[`integer`](../../data-types.md) | Identifier of the contact. Read-only ||
||**HONORIFIC**
[`crm_status`](../data-types.md) | Salutation.

You can get the values of the directory using the method [crm.status.list](../status/crm-status-list.md) with a filter for `ENTITY_ID=HONORIFIC` ||
||**NAME**
[`string`](../../data-types.md) | First Name ||
||**SECOND_NAME**
[`string`](../../data-types.md) | Middle Name ||
||**LAST_NAME**
[`string`](../../data-types.md) | Last Name ||
||**PHOTO**
[`file`](../../data-types.md) | Photo ||
||**BIRTHDATE**
[`date`](../../data-types.md) | Birthdate ||
||**TYPE_ID**
[`crm_status`](../data-types.md)| Contact Type.

You can get the values of the directory using the method [crm.status.list](../status/crm-status-list.md) with a filter for `ENTITY_ID=CONTACT_TYPE` ||
||**SOURCE_ID**
[`crm_status`](../data-types.md) | Source.

You can get the values of the directory using the method [crm.status.list](../status/crm-status-list.md) with a filter for `ENTITY_ID=SOURCE`||
||**SOURCE_DESCRIPTION**
[`string`](../../data-types.md) | Additional Source Information ||
||**POST**
[`string`](../../data-types.md) | Position ||
||**COMMENTS**
[`string`](../../data-types.md) | Comment. Supports BB codes ||
||**OPENED**
[`char`](../../data-types.md) | Available to everyone. Possible values:
- `Y` — yes
- `N` — no 

Considered in the operation of access permissions for roles with "All Open" access level ||
||**EXPORT**
[`char`](../../data-types.md) | Participates in contact export. Possible values:
- `Y` — yes
- `N` — no ||
||**HAS_PHONE**
[`char`](../../data-types.md) | Is phone set. Possible values:
- `Y` — yes
- `N` — no

Read-only ||
||**HAS_EMAIL**
[`char`](../../data-types.md) | Is e-mail set. Possible values:
- `Y` — yes
- `N` — no

Read-only  ||
||**HAS_IMOL**
[`char`](../../data-types.md) | Is open channel set. Possible values:
- `Y` — yes
- `N` — no

Read-only ||
||**ASSIGNED_BY_ID**
[`user`](../../data-types.md) | Responsible ||
||**CREATED_BY_ID**
[`user`](../../data-types.md) | Created By. Read-only ||
||**MODIFY_BY_ID**
[`user`](../../data-types.md) | Modified By. Read-only ||
||**DATE_CREATE**
[`datetime`](../../data-types.md) | Creation Date. Read-only ||
||**DATE_MODIFY**
[`datetime`](../../data-types.md) | Modification Date. Read-only ||
||**COMPANY_ID**
[`crm_company`](../data-types.md) | Main company of the contact ||
||**COMPANY_IDS**
[`crm_company`](../data-types.md) | Link of the contact to companies. Multiple. 

In the methods [`crm.contact.update`](./crm-contact-update.md) and [`crm.contact.add`](./crm-contact-add.md) it is used to submit an array of companies. 

In the methods [`crm.contact.list`](./crm-contact-list.md) and [`crm.contact.get`](./crm-contact-get.md) this field is not present and you need to use [`crm.contact.company.items.get`](./company/crm-contact-company-items-get.md) to get the list of companies  ||
||**LEAD_ID**
[`crm_lead`](../data-types.md) | Identifier of the lead associated with the contact. Read-only ||
||**FACE_ID**
[`integer`](../../data-types.md) | Link to faces from the FaceID module. Read-only ||
||**UTM_SOURCE**
[`string`](../../data-types.md) | Advertising system (Google Ads, etc.) ||
||**UTM_MEDIUM**
[`string`](../../data-types.md) | Traffic type. Possible values:
- `CPC` — ads 
- `CPM` — banners  ||
||**UTM_CAMPAIGN**
[`string`](../../data-types.md) | Campaign identifier ||
||**UTM_CONTENT**
[`string`](../../data-types.md) | Campaign content. For example, for contextual ads ||
||**UTM_TERM**
[`string`](../../data-types.md) | Campaign search condition. For example, keywords for contextual advertising ||
||**LAST_ACTIVITY_TIME**
[`datetime`](../../data-types.md) | Last activity date in the timeline. Read-only ||
||**LAST_ACTIVITY_BY**
[`user`](../../data-types.md) | Author of the last activity in the timeline. Read-only ||
||**PHONE**
[`crm_multifield`](../data-types.md) | Phones. Multiple ||
||**EMAIL**
[`crm_multifield`](../data-types.md) | E-mail. Multiple ||
||**WEB**
[`crm_multifield`](../data-types.md) | Websites. Multiple ||
||**IM**
[`crm_multifield`](../data-types.md) | Messengers. Multiple ||
||**LINK**
[`crm_multifield`](../data-types.md) | Links. Multiple. System field. ||
||**UF_...**  | Custom fields. For example, `UF_CRM_25534736`. 

Depending on the account settings, contacts may have a set of custom fields of specific types. 

You can add a custom field to a contact using the method [crm.contact.userfield.add](./userfield/crm-contact-userfield-add.md)  ||
||**PARENT_ID_...** | Relationship fields. 

If there are smart processes related to contacts in the account, for each such smart process there is a field that stores the relationship between this smart process and the contact. The field itself stores the identifier of the element of that smart process. 

For example, the field `PARENT_ID_153` — relationship with the smart process `entityTypeId=153`. It stores the identifier of the element of this smart process associated with the current contact ||
|#

**Fields for External Data Sources**

If the contact is created by an external system, then:
- the field `ORIGINATOR_ID` stores the string identifier of that system
- the field `ORIGIN_ID` stores the string identifier of the contact in that external system
- the field `ORIGIN_VERSION` stores the version of the contact data in that external system

#|
|| **Name**
`type` | **Description** ||
||**ORIGINATOR_ID**
[`string`](../../data-types.md) | Identifier of the external system that is the source of data about this contact ||
||**ORIGIN_ID**
[`string`](../../data-types.md) | Identifier of the contact in the external system ||
||**ORIGIN_VERSION**
[`string`](../../data-types.md) | Version of the contact data in the external system. 

Used to protect data from accidental overwriting by the external system. 

If the data was imported and not changed in the external system, such data can be edited in the CRM without fear that the next export will lead to data overwriting ||
|#

**Deprecated Fields**

Address fields in the contact are deprecated and are used only in compatibility mode. To work with the address, use [requisites](../requisites/index.md).

#|
|| **Name**
`type` | **Description** ||
||**ADDRESS**
[`string`](../../data-types.md) | Address ||
||**ADDRESS_2**
[`string`](../../data-types.md) | Second line of the address ||
||**ADDRESS_CITY**
[`string`](../../data-types.md) | City ||
||**ADDRESS_POSTAL_CODE**
[`string`](../../data-types.md) | Postal Code ||
||**ADDRESS_REGION**
[`string`](../../data-types.md) | Region ||
||**ADDRESS_PROVINCE**
[`string`](../../data-types.md) | Province ||
||**ADDRESS_COUNTRY**
[`string`](../../data-types.md) | Country ||
||**ADDRESS_COUNTRY_CODE**
[`string`](../../data-types.md) | Country Code ||
||**ADDRESS_LOC_ADDR_ID**
[`location`](../../data-types.md) | Location Address ID ||
|#

## Error Handling

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-contact-add.md)
- [{#T}](./crm-contact-update.md)
- [{#T}](./crm-contact-get.md)
- [{#T}](./crm-contact-list.md)
- [{#T}](./crm-contact-delete.md)