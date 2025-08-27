# Create Source biconnector.source.add

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: user with access to the "Analyst's Workspace" section

The method `biconnector.source.add` creates a new data source associated with the connector.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | An object containing data to create a new source. The object format is:

```
{
    "field_1": "value_1",
    "field_2": "value_2",
    ...,
    "field_n": "value_n"
}
```

- `field_n` — field name
- `value_n` — field value

[Detailed description below](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **title***
[`string`](../../data-types.md) | Source name ||
|| **description**
[`string`](../../data-types.md) | Source description ||
|| **active**
[`boolean`](../../data-types.md) | Source activity. 
Default is `true` ||
|| **connectorId***
[`integer`](../../data-types.md) | Connector identifier, can be obtained using the methods [biconnector.connector.list](../connector/biconnector-connector-list.md) or [biconnector.connector.add](../connector/biconnector-connector-add.md) ||
|| **settings***
[`object`](../../data-types.md) | A list of parameters for authorization, passed as an object where the key is the `code` of the parameter. 
Parameters can be obtained using the methods [biconnector.connector.list](../connector/biconnector-connector-list.md) or [biconnector.connector.get](../connector/biconnector-connector-get.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"title":"CRM Source","description":"CRM data source","connectorId":123,"settings":{"login":"admin","password":"qwerty"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/biconnector.source.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"title":"CRM Source","description":"CRM data source","connectorId":123,"settings":{"login":"admin","password":"qwerty"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/biconnector.source.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'biconnector.source.add',
    		{
    			fields: {
    				"title": "CRM Source",
    				"description": "CRM data source",
    				"connectorId": 123,
    				"settings": {
    					"login": "admin",
    					"password": "qwerty"
    				}
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	result.error() ? console.error(result.error()) : console.info(result);
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
                'biconnector.source.add',
                [
                    'fields' => [
                        "title"       => "CRM Source",
                        "description" => "CRM data source",
                        "connectorId" => 123,
                        "settings"    => [
                            "login"    => "admin",
                            "password" => "qwerty"
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($response->getError()) {
            error_log($response->getError());
            echo 'Error: ' . $response->getError();
        } else {
            echo 'Success: ' . print_r($result, true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding source: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.source.add',
        {
            fields: {
                "title": "CRM Source",
                "description": "CRM data source",
                "connectorId": 123,
                "settings": {
                    "login": "admin",
                    "password": "qwerty"
                }
            }
        },
        (result) => {
            result.error() ? console.error(result.error()) : console.info(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.source.add',
        [
            'fields' => [
                'title' => 'CRM Source',
                'description' => 'CRM data source',
                'connectorId' => 123,
                'settings' => [
                    'login' => 'admin',
                    'password' => 'qwerty'
                ]
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
    "result": {
      "id": 7
    },
    "time": {
        "start": 1725013197.635808,
        "finish": 1725013198.580873,
        "duration": 0.9450650215148926,
        "processing": 0.6822988986968994,
        "date_start": "2024-08-30T12:19:57+02:00",
        "date_finish": "2024-08-30T12:19:58+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Root element of the response, contains the identifier of the created source ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **200**

```json
{
    "error": "VALIDATION_FIELDS_NOT_PROVIDED",
    "error_description": "Fields not provided."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `VALIDATION_FIELDS_NOT_PROVIDED` | Fields not provided. | Fields were not passed in the request ||
|| `VALIDATION_UNKNOWN_PARAMETERS` | Unknown parameters: #LIST_OF_PARAMS# | Unknown parameters detected: list ||
|| `VALIDATION_REQUIRED_FIELD_MISSING` | Field "#TITLE#" is required. | Required field #TITLE# was not provided ||
|| `VALIDATION_READ_ONLY_FIELD` | Field "#TITLE#" is read only. | Field #TITLE# is read-only and cannot be modified ||
|| `VALIDATION_IMMUTABLE_FIELD` | Field "#TITLE#" is immutable. | Field #TITLE# is immutable ||
|| `VALIDATION_INVALID_FIELD_TYPE` | Field "#TITLE#" must be of type #TYPE#. | Field #TITLE# must be of type #TYPE# ||
|| `CONNECTOR_NOT_FOUND` | Connector was not found. | Connector not found ||
|| `SOURCE_CREATE_CONNECTION_ERROR` | Cannot create connection. | Error creating connection ||
|| `SOURCE_UPDATE_CONNECTION_ERROR` | Cannot update connection. | Error updating connection ||
|| `BX_ERROR` | Cannot delete source. Delete all related datasets first. | Cannot delete source while related datasets exist ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./biconnector-source-update.md)
- [{#T}](./biconnector-source-get.md)
- [{#T}](./biconnector-source-list.md)
- [{#T}](./biconnector-source-delete.md)
- [{#T}](./biconnector-source-fields.md)