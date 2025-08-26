# Get Fields of CRM Item `crm.item.fields`

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with "read" access permission for CRM object items

This method retrieves a list of fields and their configuration for items of type `entityTypeId`.

{% note warning %}

Items belonging to different types of CRM objects will have different sets of fields.

{% endnote %}

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`][1] | Identifier of the [system](./index.md) or [user-defined type](./user-defined-object-types/index.md) whose fields we want to retrieve ||
|| **useOriginalUfNames**
[`boolean`][1] | This parameter controls the format of user-defined field names in the response.   
Possible values:

- `Y` — original names of user-defined fields, e.g., `UF_CRM_2_1639669411830`
- `N` — user-defined field names in camelCase, e.g., `ufCrm2_1639669411830`

Default is `N` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Retrieve the list of fields for items in the SPA with `entityTypeId = 1268`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1268,"useOriginalUfNames":"N"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1268,"useOriginalUfNames":"N","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.fields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.item.fields',
    		{
    			entityTypeId: 1268,
    			useOriginalUfNames: 'N',
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
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
                'crm.item.fields',
                [
                    'entityTypeId'      => 1268,
                    'useOriginalUfNames' => 'N',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            return;
        }
    
        echo 'Success: ' . print_r($result->data(), true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching CRM item fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
        BX24.callMethod(
            'crm.item.fields',
            {
                entityTypeId: 1268,
                useOriginalUfNames: 'N',
            },
            (result) => {
                if (result.error())
                {
                    console.error(result.error());

                    return;
                }

                console.info(result.data());
            },
        );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.fields',
        [
            'entityTypeId' => 1268,
            'useOriginalUfNames' => 'N',
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
        "fields": {
            "id": {
                "type": "integer",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "ID",
                "upperName": "ID"
            },
            "title": {
                "type": "string",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Name",
                "upperName": "TITLE"
            },
            "xmlId": {
                "type": "string",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "External Code",
                "upperName": "XML_ID"
            },
            "createdTime": {
                "type": "datetime",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Created Time",
                "upperName": "CREATED_TIME"
            },
            "updatedTime": {
                "type": "datetime",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Updated Time",
                "upperName": "UPDATED_TIME"
            },
            "createdBy": {
                "type": "user",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Created By",
                "upperName": "CREATED_BY"
            },
            "updatedBy": {
                "type": "user",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Updated By",
                "upperName": "UPDATED_BY"
            },
            "assignedById": {
                "type": "user",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Responsible",
                "upperName": "ASSIGNED_BY_ID"
            },
            "opened": {
                "type": "boolean",
                "isRequired": true,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Available to All",
                "upperName": "OPENED"
            },
            "webformId": {
                "type": "integer",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Created by CRM Form",
                "upperName": "WEBFORM_ID"
            },
            "begindate": {
                "type": "date",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Start Date",
                "upperName": "BEGINDATE"
            },
            "closedate": {
                "type": "date",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "End Date",
                "upperName": "CLOSEDATE"
            },
            "companyId": {
                "type": "crm_company",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Company",
                "settings": {
                    "parentEntityTypeId": 4
                },
                "upperName": "COMPANY_ID"
            },
            "contactId": {
                "type": "crm_contact",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "isDeprecated": true,
                "title": "Contact",
                "upperName": "CONTACT_ID"
            },
            "contactIds": {
                "type": "crm_contact",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": true,
                "isDynamic": false,
                "title": "CONTACT_IDS",
                "upperName": "CONTACT_IDS"
            },
            "contacts": {
                "type": "crm_contact",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": true,
                "isDynamic": false,
                "title": "CONTACTS",
                "upperName": "CONTACTS"
            },
            "observers": {
                "type": "user",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": true,
                "isDynamic": false,
                "title": "Observers",
                "upperName": "OBSERVERS"
            },
            "categoryId": {
                "type": "crm_category",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Sales Funnel",
                "upperName": "CATEGORY_ID"
            },
            "movedTime": {
                "type": "datetime",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Moved Time",
                "upperName": "MOVED_TIME"
            },
            "movedBy": {
                "type": "user",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Moved By",
                "upperName": "MOVED_BY"
            },
            "stageId": {
                "type": "crm_status",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "statusType": "DYNAMIC_1268_STAGE_52",
                "title": "Stage",
                "upperName": "STAGE_ID"
            },
            "previousStageId": {
                "type": "crm_status",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "statusType": "DYNAMIC_1268_STAGE_52",
                "title": "Previous Stage",
                "upperName": "PREVIOUS_STAGE_ID"
            },
            "sourceId": {
                "type": "crm_status",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "statusType": "SOURCE",
                "title": "Source",
                "upperName": "SOURCE_ID"
            },
            "sourceDescription": {
                "type": "text",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Additional Info about Source",
                "upperName": "SOURCE_DESCRIPTION"
            },
            "currencyId": {
                "type": "crm_currency",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Currency",
                "upperName": "CURRENCY_ID"
            },
            "isManualOpportunity": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Manual Opportunity Mode",
                "upperName": "IS_MANUAL_OPPORTUNITY"
            },
            "opportunity": {
                "type": "double",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Amount",
                "upperName": "OPPORTUNITY"
            },
            "taxValue": {
                "type": "double",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Tax Amount",
                "upperName": "TAX_VALUE"
            },
            "mycompanyId": {
                "type": "crm_company",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Your Company Details",
                "settings": {
                    "isMyCompany": true,
                    "parentEntityTypeId": 4,
                    "isEmbeddedEditorEnabled": true
                },
                "upperName": "MYCOMPANY_ID"
            },
            "lastActivityBy": {
                "type": "user",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Last Activity By",
                "upperName": "LAST_ACTIVITY_BY"
            },
            "lastActivityTime": {
                "type": "datetime",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Last Activity",
                "upperName": "LAST_ACTIVITY_TIME"
            },
            "parentId1": {
                "type": "crm_entity",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Lead",
                "settings": {
                    "parentEntityTypeId": 1
                },
                "upperName": "PARENT_ID_1"
            },
            "parentId2": {
                "type": "crm_entity",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Deal",
                "settings": {
                    "parentEntityTypeId": 2
                },
                "upperName": "PARENT_ID_2"
            },
            "parentId1248": {
                "type": "crm_entity",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "SPA #16",
                "settings": {
                    "parentEntityTypeId": 1248
                },
                "upperName": "PARENT_ID_1248"
            }
        }
    },
    "time": {
        "start": 1721038185.626335,
        "finish": 1721038186.072804,
        "duration": 0.4464690685272217,
        "processing": 0.17344903945922852,
        "date_start": "2024-07-15T12:09:45+02:00",
        "date_finish": "2024-07-15T12:09:46+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`][1] | Root element of the response. Contains a single key `fields` ||
|| **fields**
[`object`][1] | Object in the format:
```
{
    field_1: value_1,
    field_2: value_2,
    ...
    field_n: value_n,
}
```

where: 
- `field_n` — field of the item
- `value_n` — information about the field in the format [`crm_rest_field_description`](../data-types.md#crm_rest_field_description)

||
|| **time**
[`time`][1]   | Information about the request execution time ||
|#

{% note info " " %}

By default, user-defined field names are returned in camelCase, e.g., `ufCrm2_1639669411830`.
When passing the parameter `useOriginalUfNames` with the value `Y`, user-defined fields will be returned with original names, e.g., `UF_CRM_2_1639669411830`.

{% endnote %}

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "NOT_FOUND", 
    "error_description": "SPA not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}


### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403`      | `allowed_only_intranet_user` | Action allowed only for intranet users | User is not an intranet user                 ||
|| `400`      | `NOT_FOUND` | SPA not found                          | Occurs when an invalid `entityTypeId` is passed              ||
|| `400`      | `ACCESS_DENIED` | You do not have permission to view this item        | User does not have read access permission for items of type `entityTypeId` ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](crm-item-add.md)
- [{#T}](crm-item-update.md)
- [{#T}](crm-item-get.md)
- [{#T}](crm-item-list.md)
- [{#T}](crm-item-delete.md)

[1]: ../data-types.md