# Get Requisite Template Fields by ID crm.requisite.preset.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method returns the requisite template by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the requisite template. Can be obtained using the method [`crm.requisite.preset.list`](./crm-requisite-preset-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":347}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.preset.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":347,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.preset.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.requisite.preset.get",
    		{ id: 347 }    // Identifier of the requisite template.
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
                'crm.requisite.preset.get',
                [
                    'id' => 347,
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
        echo 'Error getting requisite preset: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.preset.get",
        { id: 347 },    // Identifier of the requisite template.
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
        'crm.requisite.preset.get',
        [
            'id' => 347
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
        "ID": "347",
        "ENTITY_TYPE_ID": "8",
        "COUNTRY_ID": "1",
        "DATE_CREATE": "2024-05-24T15:45:44+02:00",
        "DATE_MODIFY": "",
        "CREATED_BY_ID": "1",
        "MODIFY_BY_ID": null,
        "NAME": "Individual Entrepreneur",
        "XML_ID": "EXAMPLE_COMPANY__VALUE_1",
        "ACTIVE": "Y",
        "SORT": "520"
    },
    "time": {
        "start": 1716558423.336056,
        "finish": 1716558424.695338,
        "duration": 1.3592820167541504,
        "processing": 0.06150317192077637,
        "date_start": "2024-05-24T15:47:03+02:00",
        "date_finish": "2024-05-24T15:47:04+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
`object`| An object containing the values of the template fields ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

### Description of Requisite Template Fields

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the requisite. Automatically created and unique within the account ||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the parent object type.

The identifiers of CRM object types are provided by the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) 
||
|| **COUNTRY_ID**
[`integer`](../../../data-types.md) | Identifier of the country corresponding to the requisite template field set (for available values see the method [crm.requisite.preset.countries](./crm-requisite-preset-countries.md)) ||
|| **DATE_CREATE**
[`datetime`](../../../data-types.md) | Creation date ||
|| **DATE_MODIFY**
[`datetime`](../../../data-types.md) | Modification date. Contains an empty string if the template has not been changed since creation ||
|| **CREATED_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who created the requisite ||
|| **MODIFY_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who modified the requisite ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the requisite ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key. Used for exchange operations. Identifier of the external information base object. 

The purpose of the field may change by the final developer. 

Each application ensures the uniqueness of values in this field. It is recommended to use a unique prefix to avoid collisions with other applications. 

Values of the form `#CRM_REQUISITE_PRESET_DEF_...` are reserved in CRM for identifying templates that are used by default. These identifiers should not be used for your purposes, as this may lead to logic violations ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity status. Uses values `Y` or `N`. Determines the availability of the template in the selection list when adding requisites ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "The Preset with ID '347' is not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `The Preset with ID '347' is not found` | The template with the specified identifier was not found ||
|| `Access denied` | Insufficient access permissions to retrieve the template ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-preset-add.md)
- [{#T}](./crm-requisite-preset-update.md)
- [{#T}](./crm-requisite-preset-countries.md)
- [{#T}](./crm-requisite-preset-list.md)
- [{#T}](./crm-requisite-preset-delete.md)
- [{#T}](./crm-requisite-preset-fields.md)