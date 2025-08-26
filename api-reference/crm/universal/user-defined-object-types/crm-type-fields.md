# Get Custom Type Fields crm.type.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with administrative access to the CRM section

This method retrieves information about the custom fields of the smart process settings.

No parameters.

## Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

Get information about the smart process fields

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.type.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/crm.type.fields?auth=**put_access_token_here**
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.type.fields',
    		{}
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
                'crm.type.fields',
                []
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
        echo 'Error fetching CRM type fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.type.fields',
        {},
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
        'crm.type.fields',
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
                "isRequired": true,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Title",
                "upperName": "TITLE"
            },
            "code": {
                "type": "string",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Symbolic Code",
                "upperName": "CODE"
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
            "entityTypeId": {
                "type": "integer",
                "isRequired": true,
                "isReadOnly": false,
                "isImmutable": true,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Type Identifier",
                "upperName": "ENTITY_TYPE_ID"
            },
            "customSectionId": {
                "type": "integer",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": true,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Digital Workspace",
                "upperName": "CUSTOM_SECTION_ID"
            },
            "isCategoriesEnabled": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Use custom funnels and sales tunnels in the smart process",
                "upperName": "IS_CATEGORIES_ENABLED"
            },
            "isStagesEnabled": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Use custom stages and Kanban in the smart process",
                "upperName": "IS_STAGES_ENABLED"
            },
            "isBeginCloseDatesEnabled": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Fields 'Start Date' and 'End Date'",
                "upperName": "IS_BEGIN_CLOSE_DATES_ENABLED"
            },
            "isClientEnabled": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Field 'Client'",
                "upperName": "IS_CLIENT_ENABLED"
            },
            "isUseInUserfieldEnabled": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Use in user field",
                "upperName": "IS_USE_IN_USERFIELD_ENABLED"
            },
            "isLinkWithProductsEnabled": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Link with catalog products",
                "upperName": "IS_LINK_WITH_PRODUCTS_ENABLED"
            },
            "isMycompanyEnabled": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Field 'Your Company Details'",
                "upperName": "IS_MYCOMPANY_ENABLED"
            },
            "isDocumentsEnabled": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Document Printing",
                "upperName": "IS_DOCUMENTS_ENABLED"
            },
            "isSourceEnabled": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Fields 'Source' and 'Additional Information about Source'",
                "upperName": "IS_SOURCE_ENABLED"
            },
            "isObserversEnabled": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Field 'Observers'",
                "upperName": "IS_OBSERVERS_ENABLED"
            },
            "isRecyclebinEnabled": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Use Recycle Bin",
                "upperName": "IS_RECYCLEBIN_ENABLED"
            },
            "isAutomationEnabled": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Use Automation rules and triggers in the smart process",
                "upperName": "IS_AUTOMATION_ENABLED"
            },
            "isBizProcEnabled": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Use business process designer in the smart process",
                "upperName": "IS_BIZ_PROC_ENABLED"
            },
            "isSetOpenPermissions": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Make new funnels available to everyone",
                "upperName": "IS_SET_OPEN_PERMISSIONS"
            },
            "createdTime": {
                "type": "datetime",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Creation Date",
                "upperName": "CREATED_TIME"
            },
            "updatedTime": {
                "type": "datetime",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Modification Date",
                "upperName": "UPDATED_TIME"
            },
            "updatedBy": {
                "type": "user",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Modified By",
                "upperName": "UPDATED_BY"
            }
        }
    },
    "time": {
        "start": 1720436891.554676,
        "finish": 1720436894.393655,
        "duration": 2.8389790058135986,
        "processing": 0.20139384269714355,
        "date_start": "2024-07-08T13:08:11+02:00",
        "date_finish": "2024-07-08T13:08:14+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Title**
`type` | **Description** ||
|| **result**
[`object`][1] | Root element of the response. Contains an object with a single key `fields` ||
|| **fields**
[`object`][1] | Object in the format: `{ field_1: value_1, field_2: value_2, ... , field_n: value_n }`, where `field_n` — fields of the smart process settings, and `value_n` — object of type [`crm_rest_field_description`](../../data-types.md#crm_rest_field_description) ||
|| **time**
[`time`][1] | Object containing information about the request execution time  ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `allowed_only_intranet_user` | Action allowed only for intranet users  | Occurs if the user is not an intranet user ||
|| `400` | `ACCESS_DENIED` | Access Denied | Occurs if the user does not have administrative rights in CRM ||
|#

{% include [system errors](./../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-type-add.md)
- [{#T}](./crm-type-update.md)
- [{#T}](./crm-type-get.md)
- [{#T}](./crm-type-get-by-entity-type-id.md)
- [{#T}](./crm-type-list.md)
- [{#T}](./crm-type-delete.md)

[1]: ../../../data-types.md