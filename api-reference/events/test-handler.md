# How to Test Your Handler for Processing Bitrix24 Events

After registering the handler ONAPPTEST, the method `event.test` is called manually. This triggers the specified event and allows you to verify that the handler is indeed capable of receiving event data.

## Step 1

Create a file named handler.php on your server. Ensure that it is accessible from the internet. Next to the file, create a folder named \log.

Code for the handler.php file.

{% include [Example Notes](../../_includes/examples.md) %}

{% list tabs %}

- PHP

    ```php
    <?php
    file_put_contents(
        __DIR__ . '/log/' . time() . '.txt',
        var_export($_REQUEST, true)
    );
    ```

{% endlist %}

## Step 2

Register the event by specifying the path to the file created in Step 1 in the `handler` field.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"event":"ONAPPTEST","handler":"https://example.com/handler.php"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/event.bind
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"event":"ONAPPTEST","handler":"https://example.com/handler.php","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/event.bind
    ```

- JS

    ```javascript
    BX24.callMethod(
        'event.bind',
        {
            'event': 'ONAPPTEST',
            'handler': 'https://example.com/handler.php'
        },
        function(eventBind) {
            if (eventBind.data()) {
                console.log('event bind successful');
            }
        }
    );
    ```

- PHP

    ```php
    <?php
    $eventBind = CRest::call(
        'event.bind',
        [
            'event' => 'ONAPPTEST',
            'handler' => 'https://example.com/handler.php'
        ]
    );
    if($eventBind['result'])
    {
        echo 'event bind successful';
    }
    ?>
    ```

{% endlist %}

## Step 3

Trigger the event by calling the method with arbitrary data.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"any":"data"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/event.test
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"any":"data","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/event.test
    ```

- JS

    ```javascript
    BX24.callMethod(
        'event.test',
        {
            'any': 'data'
        },
        function(result) {
            if (result.data()) {
                console.log('successful');
            }
        }
    );
    ```

- PHP

    ```php
    <?php
    $result = CRest::call(
        'event.test',
        [
            'any' => 'data'
        ]
    );
    if($result['result'])
    {
        echo 'successful';
    }
    ?>
    ```

{% endlist %}

## Result

Upon successful invocation, a file with standard event data is created in the \log folder.

{% list tabs %}

- PHP

    ```php
    array (
        'event' => 'ONAPPTEST',
        'data' => 
        array (
            'QUERY' => 
            array (
                'any' => 'data',
            ),
            'LANGUAGE_ID' => 'en',
        ),
        'ts' => '1573120286',
        'auth' => array (...)
    )
    ```

{% endlist %}