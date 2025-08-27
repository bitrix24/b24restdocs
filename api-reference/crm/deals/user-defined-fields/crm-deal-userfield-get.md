# Get Custom Deal Field by ID crm.deal.userfield.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

The method `crm.deal.userfield.get` returns a custom deal field by its identifier.

## Method Parameters

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the custom field associated with the deal.

The identifier can be obtained using the methods [crm.deal.userfield.add](./crm-deal-userfield-add.md) or [crm.deal.userfield.list](./crm-deal-userfield-list.md) ||
|#

## Code Examples

{% include [Example Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":399}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.userfield.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":399,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.userfield.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.deal.userfield.get',
    		{
    			id: 399,
    		}
    	);
    	
    	const result = response.getData().result;
    	result.error()
    		? console.error(result.error())
    		: console.info(result)
    	;
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
                'crm.deal.userfield.get',
                [
                    'id' => 399,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting deal user field: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.deal.userfield.get',
        {
            id: 399,
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.userfield.get',
        [
            'id' => 399
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
        "ID": "6997",
        "ENTITY_ID": "CRM_DEAL",
        "FIELD_NAME": "UF_CRM_HELLO_WORLD",
        "USER_TYPE_ID": "string",
        "XML_ID": null,
        "SORT": "2000",
        "MULTIPLE": "Y",
        "MANDATORY": "N",
        "SHOW_FILTER": "N",
        "SHOW_IN_LIST": "Y",
        "EDIT_IN_LIST": "N",
        "IS_SEARCHABLE": "N",
        "SETTINGS": {
            "SIZE": 20,
            "ROWS": 10,
            "REGEXP": "",
            "MIN_LENGTH": 0,
            "MAX_LENGTH": 0,
            "DEFAULT_VALUE": "Hello, World! Default value (changed)"
        },
        "EDIT_FORM_LABEL": {
            "ar": "",
            "br": "",
            "de": "Hallo, Welt! Bearbeiten (geändert)",
            "en": "Hello, World! Edit (changed)",
            "fr": "",
            "hi": "",
            "id": "",
            "in": "",
            "it": "",
            "ja": "",
            "kz": "",
            "la": "",
            "ms": "",
            "pl": "",
            "ru": "",
            "sc": "",
            "tc": "",
            "th": "",
            "tr": "",
            "ua": "",
            "vn": ""
        },
        "LIST_COLUMN_LABEL": {
            "ar": "",
            "br": "",
            "de": "Hallo, Welt! Spalte (geändert)",
            "en": "Hello, World! Column (changed)",
            "fr": "",
            "hi": "",
            "id": "",
            "in": "",
            "it": "",
            "ja": "",
            "kz": "",
            "la": "",
            "ms": "",
            "pl": "",
            "ru": "",
            "sc": "",
            "tc": "",
            "th": "",
            "tr": "",
            "ua": "",
            "vn": ""
        },
        "LIST_FILTER_LABEL": {
            "ar": "",
            "br": "",
            "de": "Hallo, Welt! Filter (geändert)",
            "en": "Hello, World! Filter (changed)",
            "fr": "",
            "hi": "",
            "id": "",
            "in": "",
            "it": "",
            "ja": "",
            "kz": "",
            "la": "",
            "ms": "",
            "pl": "",
            "ru": "",
            "sc": "",
            "tc": "",
            "th": "",
            "tr": "",
            "ua": "",
            "vn": ""
        },
        "ERROR_MESSAGE": {
            "ar": "",
            "br": "",
            "de": "Hallo, Welt! Fehler (geändert)",
            "en": "Hello, World! Error (changed)",
            "fr": "",
            "hi": "",
            "id": "",
            "in": "",
            "it": "",
            "ja": "",
            "kz": "",
            "la": "",
            "ms": "",
            "pl": "",
            "ru": "",
            "sc": "",
            "tc": "",
            "th": "",
            "tr": "",
            "ua": "",
            "vn": ""
        },
        "HELP_MESSAGE": {
            "ar": "",
            "br": "",
            "de": "Hallo, Welt! Hilfe (geändert)",
            "en": "Hello, World! Help (changed)",
            "fr": "",
            "hi": "",
            "id": "",
            "in": "",
            "it": "",
            "ja": "",
            "kz": "",
            "la": "",
            "ms": "",
            "pl": "",
            "ru": "",
            "sc": "",
            "tc": "",
            "th": "",
            "tr": "",
            "ua": "",
            "vn": ""
        }
    },
    "time": {
        "start": 1753790529.430936,
        "finish": 1753790529.487882,
        "duration": 0.05694580078125,
        "processing": 0.0039789676666259766,
        "date_start": "2025-07-29T15:02:09+02:00",
        "date_finish": "2025-07-29T15:02:09+02:00",
        "operating_reset_at": 1753791129,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | The root element of the response, contains information about the custom field. The final list of fields depends on the field type, detailed descriptions of the fields can be found in the method [crm.deal.userfield.add](./crm-deal-userfield-add.md) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "ID is not defined or invalid."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `403` | `Access denied` | Occurs when:
- the user does not have administrative rights
- the user attempts to access a custom field not linked to deals ||
|| `400` | `ID is not defined or invalid` | The provided `id` is less than or equal to zero, or not provided at all ||
|| `ERROR_NOT_FOUND` | `The entity with ID 'id' is not found` | The custom field with the provided `id` was not found ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-userfield-add.md)
- [{#T}](./crm-deal-userfield-update.md)
- [{#T}](./crm-deal-userfield-list.md)
- [{#T}](./crm-deal-userfield-delete.md)