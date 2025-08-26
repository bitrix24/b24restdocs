# Update Requisite Template crm.requisite.preset.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates the requisite template.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the requisite template to be updated. Can be obtained using the method [`crm.requisite.preset.list`](./crm-requisite-preset-list.md) ||
|| **fields***
[`array`](../../../data-types.md) | Set of template fields — an object of the form `{"field": "value"[, ...]}`, the values of which need to be changed ||
|#

### fields Parameter

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME***
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

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":347,"fields":{"NAME":"IP (archive)","ACTIVE":"N"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.preset.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":347,"fields":{"NAME":"IP (archive)","ACTIVE":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.preset.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.requisite.preset.update",
    		{
    			id: 347,    // Identifier of the template to be updated.
    			fields:
    			{
    				"NAME": "IP (archive)",
    				"ACTIVE": "N"
    			}
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
                'crm.requisite.preset.update',
                [
                    'id' => 347,
                    'fields' => [
                        'NAME' => 'IP (archive)',
                        'ACTIVE' => 'N',
                    ],
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
        echo 'Error updating requisite preset: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.preset.update",
        {
            id: 347,    // Identifier of the template to be updated.
            fields:
            {
                "NAME": "IP (archive)",
                "ACTIVE": "N"
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.preset.update',
        [
            'id' => 347,
            'fields' =>
            [
                'NAME' => 'IP (archive)',
                'ACTIVE' => 'N'
            ]
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
    "result": true,
    "time": {
        "start": 1716566193.821566,
        "finish": 1716566194.075617,
        "duration": 0.25405097007751465,
        "processing": 0.03606295585632324,
        "date_start": "2024-05-24T17:56:33+02:00",
        "date_finish": "2024-05-24T17:56:34+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of updating the template:
- `true` — template updated
- `false` — template not updated
||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
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
|| `The Preset with ID '347' is not found` | Template with the specified identifier not found ||
|| `Access denied` | Insufficient access permissions to update the template ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-preset-add.md)
- [{#T}](./crm-requisite-preset-countries.md)
- [{#T}](./crm-requisite-preset-get.md)
- [{#T}](./crm-requisite-preset-list.md)
- [{#T}](./crm-requisite-preset-delete.md)
- [{#T}](./crm-requisite-preset-fields.md)