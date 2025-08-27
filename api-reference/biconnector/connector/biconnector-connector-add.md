# Create Connector biconnector.connector.add

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: user with access to the "Analyst's workspace" section

The method `biconnector.connector.add` creates a new connector that allows integrating external data sources into Bitrix24.

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | An object containing data to create a new connector. The object format: 

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
[`string`](../../data-types.md) | Connector name ||
|| **logo***
[`string`](../../data-types.md) | Connector logo. Can be provided as a link to an image or a base64 formatted string, for example `data:image/svg+xml;base64,PHN2ZyB3...` ||
|| **description**
[`string`](../../data-types.md) | Connector description ||
|| **urlCheck***
[`string`](../../data-types.md) | Connector endpoint for availability check, [(detailed description)](./index.md#urlCheck) ||
|| **urlTableList***
[`string`](../../data-types.md) | Connector endpoint for retrieving the list of tables, [(detailed description)](./index.md#urlTableList) ||
|| **urlTableDescription***
[`string`](../../data-types.md) | Connector endpoint for retrieving the description of a specific table, [(detailed description)](./index.md#urlTableDescription) ||
|| **urlData***
[`string`](../../data-types.md) | Connector endpoint for retrieving data from the selected table, [(detailed description)](./index.md#urlData) ||
|| **settings***
[`array`](../../data-types.md) | List of connection parameters, [(detailed description)](./index.md#settings) ||
|| **sort**
[`int`](../../data-types.md) | Connector sorting parameter. Default value is `100` ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{
             "fields": {
                 "title": "SUPER REST CONNECTOR",
                 "logo": "data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyMiAyMiIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxjaXJjbGUgY3g9IjExIiBjeT0iMTEiIHI9IjEwIiBmaWxsPSIjRkYzQjNCIiAvPgoJPHRleHQgeD0iMTEiIHk9IjEzIiBmb250LWZhbWlseT0iQXJpYWwsIHNhbnMtc2VyaWYiIGZvbnQtc2l6ZT0iNiIgZmlsbD0iI0ZGRkZGRiIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC13ZWlnaHQ9ImJvbGQiPlJFU1Q8L3RleHQ+Cjwvc3ZnPg==",
                 "description": "Connector with token",
                 "urlCheck": "http://example.com/api/check",
                 "urlTableList": "http://example.com/api/table_list",
                 "urlTableDescription": "http://example.com/api/table_description",
                 "urlData": "http://example.com/api/data",
                 "settings": [
                    {
                        "name": "Login",
                        "type": "STRING",
                        "code": "login"
                    },
                    {
                        "name": "Password",
                        "type": "STRING",
                        "code": "password"
                    }
                 ],
                 "sort": 100
             }
             }' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/biconnector.connector.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{
             "fields": {
                 "title": "SUPER REST CONNECTOR",
                 "logo": "data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyMiAyMiIgZmlsbD0ibm9uZSIgeG1zbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxjaXJjbGUgY3g9IjExIiBjeT0iMTEiIHI9IjEwIiBmaWxsPSIjRkYzQjNCIiAvPgoJPHRleHQgeD0iMTEiIHk9IjEzIiBmb250LWZhbWlseT0iQXJpYWwsIHNhbnMtc2VyaWYiIGZvbnQtc2l6ZT0iNiIgZmlsbD0iI0ZGRkZGRiIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC13ZWlnaHQ9ImJvbGQiPlJFU1Q8L3RleHQ+Cjwvc3ZnPg==",
                 "description": "Connector with token",
                 "urlCheck": "http://example.com/api/check",
                 "urlTableList": "http://example.com/api/table_list",
                 "urlTableDescription": "http://example.com/api/table_description",
                 "urlData": "http://example.com/api/data",
                 "settings": [
                    {
                        "name": "Login",
                        "type": "STRING",
                        "code": "login"
                    },
                    {
                        "name": "Password",
                        "type": "STRING",
                        "code": "password"
                    }
                 ],
                 "sort": 100
             },
             "auth": "**put_access_token_here**"
             }' \
         https://**put_your_bitrix24_address**/rest/biconnector.connector.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'biconnector.connector.add',
    		{
    			fields: {
    				"title": "SUPER REST CONNECTOR",
    				"logo": "data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyMiAyMiIgZmlsbD0ibm9uZSIgeG1zbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxjaXJjbGUgY3g9IjExIiBjeT0iMTEiIHI9IjEwIiBmaWxsPSIjRkYzQjNCIiAvPgoJPHRleHQgeD0iMTEiIHk9IjEzIiBmb250LWZhbWlseT0iQXJpYWwsIHNhbnMtc2VyaWYiIGZvbnQtc2l6ZT0iNiIgZmlsbD0iI0ZGRkZGRiIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC13ZWlnaHQ9ImJvbGQiPlJFU1Q8L3RleHQ+Cjwvc3ZnPg==",
    				"description": "Connector with token",
    				"urlCheck": "http://example.com/api/check",
    				"urlTableList": "http://example.com/api/table_list",
    				"urlTableDescription": "http://example.com/api/table_description",
    				"urlData": "http://example.com/api/data",
    				"settings": [
    					{
    						"name": "Login",
    						"type": "STRING",
    						"code": "login"
    					},
    					{
    						"name": "Password",
    						"type": "STRING",
    						"code": "password"
    					}
    				],
    				"sort": 100
    			},
    		}
    	);
    	
    	const result = response.getData().result;
    	result.error()
    		? console.error(result.error())
    		: console.info(result.data());
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
                'biconnector.connector.add',
                [
                    'fields' => [
                        "title"               => "SUPER REST CONNECTOR",
                        "logo"                => "data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyMiAyMiIgZmlsbD0ibm9uZSIgeG1zbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxjaXJjbGUgY3g9IjExIiBjeT0iMTEiIHI9IjEwIiBmaWxsPSIjRkYzQjNCIiAvPgoJPHRleHQgeD0iMTEiIHk9IjEzIiBmb250LWZhbWlseT0iQXJpYWwsIHNhbnMtc2VyaWYiIGZvbnQtc2l6ZT0iNiIgZmlsbD0iI0ZGRkZGRiIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC13ZWlnaHQ9ImJvbGQiPlJFU1Q8L3RleHQ+Cjwvc3ZnPg==",
                        "description"          => "Connector with token",
                        "urlCheck"             => "http://example.com/api/check",
                        "urlTableList"         => "http://example.com/api/table_list",
                        "urlTableDescription"  => "http://example.com/api/table_description",
                        "urlData"              => "http://example.com/api/data",
                        "settings"             => [
                            [
                                "name" => "Login",
                                "type" => "STRING",
                                "code" => "login"
                            ],
                            [
                                "name" => "Password",
                                "type" => "STRING",
                                "code" => "password"
                            ]
                        ],
                        "sort"                => 100
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding connector: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.connector.add',
        {
            fields: {
                "title": "SUPER REST CONNECTOR",
                "logo": "data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyMiAyMiIgZmlsbD0ibm9uZSIgeG1zbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxjaXJjbGUgY3g9IjExIiBjeT0iMTEiIHI9IjEwIiBmaWxsPSIjRkYzQjNCIiAvPgoJPHRleHQgeD0iMTEiIHk9IjEzIiBmb250LWZhbWlseT0iQXJpYWwsIHNhbnMtc2VyaWYiIGZvbnQtc2l6ZT0iNiIgZmlsbD0iI0ZGRkZGRiIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC13ZWlnaHQ9ImJvbGQiPlJFU1Q8L3RleHQ+Cjwvc3ZnPg==",
                "description": "Connector with token",
                "urlCheck": "http://example.com/api/check",
                "urlTableList": "http://example.com/api/table_list",
                "urlTableDescription": "http://example.com/api/table_description",
                "urlData": "http://example.com/api/data",
                "settings": [
                    {
                        "name": "Login",
                        "type": "STRING",
                        "code": "login"
                    },
                    {
                        "name": "Password",
                        "type": "STRING",
                        "code": "password"
                    }
                ],
                "sort": 100
            },
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data());
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.connector.add',
        [
            'fields' => [
                'title' => 'SUPER REST CONNECTOR',
                'logo' => 'data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjIiIGhlaWdodD0iMjIiIHZpZXdCb3g9IjAgMCAyMiAyMiIgZmlsbD0ibm9uZSIgeG1zbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KCTxjaXJjbGUgY3g9IjExIiBjeT0iMTEiIHI9IjEwIiBmaWxsPSIjRkYzQjNCIiAvPgoJPHRleHQgeD0iMTEiIHk9IjEzIiBmb250LWZhbWlseT0iQXJpYWwsIHNhbnMtc2VyaWYiIGZvbnQtc2l6ZT0iNiIgZmlsbD0iI0ZGRkZGRiIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC13ZWlnaHQ9ImJvbGQiPlJFU1Q8L3RleHQ+Cjwvc3ZnPg==',
                'description' => 'Connector with token',
                'urlCheck' => 'http://example.com/api/check',
                'urlTableList' => 'http://example.com/api/table_list',
                'urlTableDescription' => 'http://example.com/api/table_description',
                'urlData' => 'http://example.com/api/data',
                'settings' => [
                    [
                        'name' => 'Login',
                        'type' => 'STRING',
                        'code' => 'login'
                    ],
                    [
                        'name' => 'Password',
                        'type' => 'STRING',
                        'code' => 'password'
                    ]
                ],
                'sort' => 100
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
      "id": 4
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
[`integer`](../../data-types.md) | Root element of the response, contains the identifier of the created connector ||
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
|| `VALIDATION_FIELDS_NOT_PROVIDED` | Fields not provided | Fields were not passed in the request ||
|| `VALIDATION_UNKNOWN_PARAMETERS` | Unknown parameters: #LIST_OF_PARAMS# | Unknown parameters detected: list ||
|| `VALIDATION_REQUIRED_FIELD_MISSING` | Field "#TITLE#" is required. | Required field #TITLE# was not provided ||
|| `VALIDATION_READ_ONLY_FIELD` | Field "#TITLE#" is read only. | Field #TITLE# is read-only and cannot be modified ||
|| `VALIDATION_IMMUTABLE_FIELD` | Field "#TITLE#" is immutable. | Field #TITLE# is immutable ||
|| `VALIDATION_INVALID_FIELD_TYPE` | Field "#TITLE#" must be of type #TYPE#. | Field #TITLE# must be of type #TYPE# ||
|| `VALIDATION_SETTINGS_MISSING_REQUIRED_FIELDS` | Settings must include "type", "name" and "code" fields. | Settings must include `type`, `name`, and `code` fields ||
|| `VALIDATION_SETTINGS_NAME_TOO_LONG` | Parameter "name" must be less than 512 characters. | The value of the parameter `name` must not exceed 512 characters ||
|| `VALIDATION_SETTINGS_CODE_TOO_LONG` | Parameter "code" must be less than 512 characters. | The value of the parameter `code` must not exceed 512 characters ||
|| `VALIDATION_SETTINGS_INVALID_TYPE` | Parameter "type" is not correct. | Invalid value for parameter `type` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./biconnector-connector-update.md)
- [{#T}](./biconnector-connector-get.md)
- [{#T}](./biconnector-connector-list.md)
- [{#T}](./biconnector-connector-delete.md)
- [{#T}](./biconnector-connector-fields.md)