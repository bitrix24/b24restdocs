# Change settings for the connector imconnector.connector.data.set

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- no success response
- no error response
  
{% endnote %}

{% endif %}

> Scope: [`imconnector`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method sets data for the REST connector.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **CONNECTOR** | Connector identifier ||
|| **LINE** | Identifier of the line to which the connector is attached ||
|| **DATA** | Array with data to save:
- `id` — identifier of the account connected to this connector
- `url` and `url_im` — links to the chat. `url_im` is used in the widget, but if not set, `url` will be used
- `name` — name of the channel that will be displayed in the widget ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CONNECTOR":"myrestconnector","LINE":1,"DATA":{"id":123,"url":"http://localhost","url_im":"http://localhost","name":"My rest connector name"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imconnector.connector.data.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CONNECTOR":"myrestconnector","LINE":1,"DATA":{"id":123,"url":"http://localhost","url_im":"http://localhost","name":"My rest connector name"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imconnector.connector.data.set
    ```

- JS

    ```js
    function connectorDataSet()
    {
        var params = {
            CONNECTOR: 'myrestconnector',
            LINE: 1,
            DATA: {
                id: 123,
                url: 'http://localhost',
                url_im: 'http://localhost',
                name: 'My rest connector name'
            }
        };
        BX24.callMethod(
            'imconnector.connector.data.set',
            params,
            function (result) {
                if (result.error())
                    alert("Error: " + result.error());
                else
                    alert("Success: " + result.data());
            }
        );
    }
    ```

- PHP

    ```php
    require_once('crest.php');

    $params = [
        'CONNECTOR' => 'myrestconnector',
        'LINE' => 1,
        'DATA' => [
            'id' => 123,
            'url' => 'http://localhost',
            'url_im' => 'http://localhost',
            'name' => 'My rest connector name'
        ]
    ];

    $result = CRest::call(
        'imconnector.connector.data.set',
        $params
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Learning 

- [{#T}](./imconnector-register.md)
- [{#T}](./imconnector-activate.md)
- [{#T}](./imconnector-deactivate.md)
- [{#T}](./imconnector-status.md)
- [{#T}](./imconnector-list.md)
- [{#T}](./imconnector-unregister.md)
- [{#T}](./imconnector-send-messages.md)
- [{#T}](./imconnector-update-messages.md)
- [{#T}](./imconnector-delete-messages.md)
- [{#T}](./imconnector-send-status-delivery.md)
- [{#T}](./imconnector-send-status-reading.md)