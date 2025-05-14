# Update the biconnector.connector.update

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: user with access to the "Analyst Workspace" section

The method `biconnector.connector.update` updates an existing connector.

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Connector identifier, can be obtained using the methods [biconnector.connector.list](./biconnector-connector-list.md) and [biconnector.connector.add](./biconnector-connector-add.md) ||
|| **fields***
[`object`](../../data-types.md) | Object containing the updated data. Object format: 

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
|| **title**
[`string`](../../data-types.md) | New connector name ||
|| **logo**
[`string`](../../data-types.md) | New connector logo. Can be passed as a link to an image or a base64 formatted string, for example `data:image/svg+xml;base64,PHN2ZyB3...` ||
|| **description**
[`string`](../../data-types.md) | New connector description ||
|| **urlCheck**
[`string`](../../data-types.md) | New endpoint for checking connector availability, [(detailed description)](./index.md#urlCheck) ||
|| **urlTableList**
[`string`](../../data-types.md) | New endpoint for retrieving the list of tables, [(detailed description)](./index.md#urlTableList) ||
|| **urlTableDescription**
[`string`](../../data-types.md) | New endpoint for retrieving the description of a specific table, [(detailed description)](./index.md#urlTableDescription) ||
|| **urlData**
[`string`](../../data-types.md) | New endpoint for retrieving data from the selected table, [(detailed description)](./index.md#urlData)  ||
|| **settings**
[`array`](../../data-types.md) | New list of connection parameters, [(detailed description)](./index.md#settings) ||
|| **sort**
[`int`](../../data-types.md) | New sorting parameter for the connector ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX.rest.callMethod(
        'biconnector.connector.update',
        {
            id: 4,
            fields: {
                "title": "UPDATED REST CONNECTOR",
                "logo": "data:image/svg+xml;base64,NEWLOGODATA",
                "description": "Updated description",
                "urlCheck": "http://example.com/api/new_check",
                "urlTableList": "http://example.com/api/new_table_list",
                "urlTableDescription": "http://example.com/api/new_table_description",
                "urlData": "http://example.com/api/new_data",
                "settings": [
                    {
                        "name": "Employee Identifier",
                        "type": "STRING",
                        "code": "id"
                    },
                    {
                        "name": "Password",
                        "type": "STRING",
                        "code": "password"
                    }
                ],
                "sort": 200
            }
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data());
        }
    );
    ```

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{
             "id": 4,
             "fields": {
                 "title": "UPDATED REST CONNECTOR",
                 "logo": "data:image/svg+xml;base64,NEWLOGODATA",
                 "description": "Updated description",
                 "urlCheck": "http://example.com/api/new_check",
                 "urlTableList": "http://example.com/api/new_table_list",
                 "urlTableDescription": "http://example.com/api/new_table_description",
                 "urlData": "http://example.com/api/new_data",
                 "settings": [
                    {
                        "name": "Employee Identifier",
                        "type": "STRING",
                        "code": "id"
                    },
                    {
                        "name": "Password",
                        "type": "STRING",
                        "code": "password"
                    }
                 ],
                 "sort": 200
             }
             }' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/biconnector.connector.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{
             "id": 4,
             "fields": {
                 "title": "UPDATED REST CONNECTOR",
                 "logo": "data:image/svg+xml;base64,NEWLOGODATA",
                 "description": "Updated description",
                 "urlCheck": "http://example.com/api/new_check",
                 "urlTableList": "http://example.com/api/new_table_list",
                 "urlTableDescription": "http://example.com/api/new_table_description",
                 "urlData": "http://example.com/api/new_data",
                 "settings": [
                    {
                        "name": "Employee Identifier",
                        "type": "STRING",
                        "code": "id"
                    },
                    {
                        "name": "Password",
                        "type": "STRING",
                        "code": "password"
                    }
                 ],
                 "sort": 200
             },
             "auth": "**put_access_token_here**"
             }' \
         https://**put_your_bitrix24_address**/rest/biconnector.connector.update
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.connector.update',
        [
            'id' => 4,
            'fields' => [
                'title' => 'UPDATED REST CONNECTOR',
                'logo' => 'data:image/svg+xml;base64,NEWLOGODATA',
                'description' => 'Updated description',
                'urlCheck' => 'http://example.com/api/new_check',
                'urlTableList' => 'http://example.com/api/new_table_list',
                'urlTableDescription' => 'http://example.com/api/new_table_description',
                'urlData' => 'http://example.com/api/new_data',
                'settings' => [
                    [
                        'name' => 'Employee Identifier',
                        'type' => 'STRING',
                        'code' => 'id'
                    ],
                    [
                        'name' => 'Password',
                        'type' => 'STRING',
                        'code' => 'password'
                    ]
                ],
                'sort' => 200
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
        "start": 1725365418.056843,
        "finish": 1725365419.671506,
        "duration": 1.6146628856658936,
        "processing": 1.3475170135498047,
        "date_start": "2024-09-03T14:10:18+02:00",
        "date_finish": "2024-09-03T14:10:19+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
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

## Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `VALIDATION_ID_NOT_PROVIDED` | `ID is missing.` | Identifier is not specified ||
|| `VALIDATION_INVALID_ID_FORMAT` | `ID has to be a positive integer.` | Invalid ID format ||
|| `VALIDATION_FIELDS_NOT_PROVIDED` | `Fields not provided.` | Fields not passed in the request ||
|| `VALIDATION_UNKNOWN_PARAMETERS` | `Unknown parameters: #LIST_OF_PARAMS#` | Unknown parameters detected: list ||
|| `VALIDATION_READ_ONLY_FIELD` | `Field "#TITLE#" is read only.` | Field #TITLE# is read-only and cannot be modified ||
|| `VALIDATION_IMMUTABLE_FIELD` | `Field "#TITLE#" is immutable.` | Field #TITLE# is immutable ||
|| `VALIDATION_INVALID_FIELD_TYPE` | `Field "#TITLE#" must be of type #TYPE#.` | Field #TITLE# must be of type #TYPE# ||
|| `CONNECTOR_NOT_FOUND` | `Connector was not found.` | Connector not found ||
|| `VALIDATION_SETTINGS_MISSING_REQUIRED_FIELDS` | `Settings must include "type", "name" and "code" fields.` | Settings must include `type`, `name`, and `code` fields ||
|| `VALIDATION_SETTINGS_INVALID_TYPE` | `Parameter "type" is not correct.` | Invalid value for parameter `type` ||
|| `VALIDATION_SETTINGS_NAME_TOO_LONG` | `Parameter "name" must be less than 512 characters.` | Value for parameter `name` must not exceed 512 characters ||
|| `VALIDATION_SETTINGS_CODE_TOO_LONG` | `Parameter "code" must be less than 512 characters.` | Value for parameter `code` must not exceed 512 characters ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./biconnector-connector-add.md)
- [{#T}](./biconnector-connector-get.md)
- [{#T}](./biconnector-connector-list.md)
- [{#T}](./biconnector-connector-delete.md)
- [{#T}](./biconnector-connector-fields.md)