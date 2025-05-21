# Get Connector by ID biconnector.connector.get

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: user with access to the "Analyst's workspace" section

The method `biconnector.connector.get` returns information about the connector by its identifier.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Connector identifier, which can be obtained using the methods [biconnector.connector.list](./biconnector-connector-list.md) and [biconnector.connector.add](./biconnector-connector-add.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX.rest.callMethod(
        'biconnector.connector.get',
        {
            id: 4,
        },
        (result) => {
            result.error() ? console.error(result.error()) : console.info(result.data());
        }
    );
    ```

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{
             "id": 4
             }' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/biconnector.connector.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{
             "id": 4,
             "auth": "**put_access_token_here**"
             }' \
         https://**put_your_bitrix24_address**/rest/biconnector.connector.get
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.connector.get',
        [
            'id' => 4
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
        "item": {
            "id": 5,
            "title": "SUPER REST CONNECTOR",
            "dateCreate": "2025-03-24 07:25:59",
            "logo": "https://masterpiecer-images.s3.amazonaws.com/5fd531dca6427c7:upscaled",
            "description": "Connector with token",
            "sort": 100,
            "urlCheck": "http://app.domain/check",
            "settings": [
                {
                    "name": "Login",
                    "code": "login",
                    "type": "STRING"
                },
                {
                    "name": "Password",
                    "code": "password",
                    "type": "STRING"
                }
            ],
            "urlData": "http://app.domain/data",
            "urlTableList": "http://app.domain/table_list",
            "urlTableDescription": "http://app.domain/table_description"
        }
    },
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
[`item`](../../data-types.md) | Root element of the response. Contains information about the connector fields. Field descriptions can be found in the article [Connector: Overview of Methods](./index.md#fields) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#                                                                         

## Error Handling

HTTP Status: **200**

```json
{
    "error": "VALIDATION_ID_NOT_PROVIDED",
    "error_description": "ID is missing."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

## Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `VALIDATION_ID_NOT_PROVIDED` | `ID is missing.` | Identifier is not specified ||
|| `VALIDATION_INVALID_ID_FORMAT` | `ID has to be a positive integer.` | Invalid ID format ||
|| `CONNECTOR_NOT_FOUND` | `Connector was not found.` | Connector not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./biconnector-connector-update.md)
- [{#T}](./biconnector-connector-add.md)
- [{#T}](./biconnector-connector-list.md)
- [{#T}](./biconnector-connector-delete.md)
- [{#T}](./biconnector-connector-fields.md)