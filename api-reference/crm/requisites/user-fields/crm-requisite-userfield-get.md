# Get User Field by ID crm.requisite.userfield.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns the user field of the requisite by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the user field. Can be obtained using the method [crm.requisite.userfield.list](./crm-requisite-userfield-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":235}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.userfield.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":235,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.userfield.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.requisite.userfield.get',
    		{
    			id: 235
    		}
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
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
                'crm.requisite.userfield.get',
                [
                    'id' => 235
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
        echo 'Error getting user field: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.userfield.get",
        {
            id: 235
        },
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
        'crm.requisite.userfield.get',
        [
            'id' => 235
        ]
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
        "ID": "235",
        "ENTITY_ID": "CRM_REQUISITE",
        "FIELD_NAME": "UF_CRM_NEWTECH_V1_STRING",
        "USER_TYPE_ID": "string",
        "XML_ID": null,
        "SORT": "100",
        "MULTIPLE": "N",
        "MANDATORY": "N",
        "SHOW_FILTER": "N",
        "SHOW_IN_LIST": "Y",
        "EDIT_IN_LIST": "Y",
        "IS_SEARCHABLE": "N",
        "SETTINGS": {
            "SIZE": 20,
            "ROWS": 1,
            "REGEXP": "",
            "MIN_LENGTH": 0,
            "MAX_LENGTH": 0,
            "DEFAULT_VALUE": ""
        },
        "EDIT_FORM_LABEL": {
            "en": "PP - String",
            "de": "PP - String"
        },
        "LIST_COLUMN_LABEL": {
            "en": "PP - String",
            "de": "PP - String"
        },
        "LIST_FILTER_LABEL": {
            "en": "PP - String",
            "de": "PP - String"
        },
        "ERROR_MESSAGE": {
            "en": "UF_CRM_NEWTECH_V1_STRING",
            "de": "UF_CRM_NEWTECH_V1_STRING"
        },
        "HELP_MESSAGE": {
            "en": "UF_CRM_NEWTECH_V1_STRING",
            "de": "UF_CRM_NEWTECH_V1_STRING"
        }
    },
    "time": {
        "start": 1717686921.82579,
        "finish": 1717686922.874864,
        "duration": 1.0490741729736328,
        "processing": 0.05396890640258789,
        "date_start": "2024-06-06T17:15:21+02:00",
        "date_finish": "2024-06-06T17:15:22+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object containing field values describing the user field of the requisite ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

### Description of User Field of the Requisite

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`int`](../../../data-types.md) | Identifier of the user field ||
|| **ENTITY_ID**
[`string`](../../../data-types.md) | Identifier of the entity to which the user field belongs. For requisites, this is always `CRM_REQUISITE` ||
|| **FIELD_NAME**
[`string`](../../../data-types.md) | Symbolic code. For requisites, it always starts with the prefix `UF_CRM_` ||
|| **USER_TYPE_ID**
[`string`](../../../data-types.md) | Data type ([`string`](../../universal/user-defined-fields/crm-userfield-types.md), [`boolean`](../../universal/user-defined-fields/crm-userfield-types.md), [`double`](../../universal/user-defined-fields/crm-userfield-types.md) or [`datetime`](../../universal/user-defined-fields/crm-userfield-types.md)) ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key. Used for exchange operations. Identifier of the object in the external information base. 

The purpose of the field may change by the final developer ||
|| **SORT**
[`int`](../../../data-types.md) | Sorting ||
|| **MULTIPLE**
[`char`](../../../data-types.md) | Multiplicity indicator. Possible values:
- `Y` — yes
- `N` — no
||
|| **MANDATORY**
[`char`](../../../data-types.md) | Mandatory indicator. Possible values:
- `Y` — yes
- `N` — no 
||
|| **SHOW_FILTER**
[`char`](../../../data-types.md) | Whether to show in the list filter. Possible values:
- `N` — do not show
- `I` — exact match
- `E` — mask
- `S` — substring 
||
|| **SHOW_IN_LIST**
[`char`](../../../data-types.md) | Whether to show in the list. Possible values:
- `Y` — yes
- `N` — no 
||
|| **EDIT_IN_LIST**
[`char`](../../../data-types.md) | Whether to allow editing by the user. Possible values:
- `Y` — yes
- `N` — no 
||
|| **IS_SEARCHABLE**
[`char`](../../../data-types.md) | Whether the field values participate in search. Possible values:
- `Y` — yes
- `N` — no 
||
|| **EDIT_FORM_LABEL**
[`string`](../../../data-types.md) | Label in the edit form ||
|| **LIST_COLUMN_LABEL**
[`string`](../../../data-types.md) | Header in the list ||
|| **LIST_FILTER_LABEL**
[`string`](../../../data-types.md) | Filter label in the list ||
|| **ERROR_MESSAGE**
[`string`](../../../data-types.md) | Error message ||
|| **HELP_MESSAGE**
[`string`](../../../data-types.md) | Help ||
|| **LIST**
[`uf_enum_element`](../../../data-types.md) | List elements. For detailed information, see the section [{#T}](../../universal/user-defined-fields/crm-userfield-enumeration-fields.md) ||
|| **SETTINGS**
[`object`](../../../data-types.md) | Additional settings (dependent on type). For detailed information, see the section [{#T}](../../universal/user-defined-fields/crm-userfield-settings-fields.md) ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "ERROR_NOT_FOUND",
    "error_description": "The entity with ID '235' is not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Code** | **Error Text** | **Description** ||
|| Empty string | `ID is not defined or invalid` | The identifier of the user field is not set or has an invalid value ||
|| `ERROR_NOT_FOUND` | `The entity with ID '235' is not found` | The user field with the specified identifier was not found ||
|| Empty string | `Access denied` | Insufficient access permissions to retrieve the user field ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-userfield-add.md)
- [{#T}](./crm-requisite-userfield-update.md)
- [{#T}](./crm-requisite-userfield-list.md)
- [{#T}](./crm-requisite-userfield-delete.md)