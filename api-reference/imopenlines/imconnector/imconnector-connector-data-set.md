# Change settings for the connector imconnector.connector.data.set

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- no response in case of success
- no response in case of error
  
{% endnote %}

{% endif %}

> Scope: [`imconnector`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method sets data for the REST connector.

## Method parameters

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

## Code examples

{% include [Note about examples](../../../_includes/examples.md) %}

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
    async function connectorDataSet()
    {
        try
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
            
            const response = await $b24.callMethod(
                'imconnector.connector.data.set',
                params
            );
            
            const result = response.getData().result;
            alert("Successfully: " + result);
        }
        catch(error)
        {
            alert("Error: " + error);
        }
    }
    ```

- PHP

    ```php
    function connectorDataSet()
    {
        try {
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
    
            $response = $b24Service
                ->core
                ->call(
                    'imconnector.connector.data.set',
                    $params
                );
    
            $result = $response
                ->getResponseData()
                ->getResult();
    
            if ($result->error()) {
                echo 'Error: ' . $result->error();
            } else {
                echo 'Successfully: ' . $result->data();
            }
    
        } catch (Throwable $e) {
            error_log($e->getMessage());
            echo 'Error setting connector data: ' . $e->getMessage();
        }
    }
    ```

- BX24.js

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
                    alert("Successfully: " + result.data());
            }
        );
    }
    ```

- PHP CRest

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

## Continue exploring 

- [{#T}](./imconnector-register.md)
- [{#T}](./imconnector-activate.md)
- [{#T}](./imconnector-status.md)
- [{#T}](./imconnector-list.md)
- [{#T}](./imconnector-unregister.md)
- [{#T}](./imconnector-send-messages.md)
- [{#T}](./imconnector-update-messages.md)
- [{#T}](./imconnector-delete-messages.md)
- [{#T}](./imconnector-send-status-delivery.md)
- [{#T}](./imconnector-send-status-reading.md)
- [{#T}](../../../tutorials/openlines/example-connector.md)