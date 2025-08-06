# How to Create an Open Channel Connector for Chat on the Site

{% note info "" %}

The example works only with application authorization. It will not work when using webhooks.

To use the example, configure the CRest class. For detailed information, read the article [Loading and Using CRest PHP SDK](../../api-reference/crest-php-sdk/index.md).

{% endnote %}

You can create an online chat on the site. When a visitor writes a message, the open channel connector sends the text to Bitrix24. An employee replies from Bitrix24 — the response is displayed on the site.

To set up the connector and create an online chat, we will perform six steps:

1. Create a file `function.php` with helper functions and a class for working with the API.

2. Create a file `install_connector.php` to register the connector. In the file, we will call the methods:

    - [imconnector.register](../../api-reference/imopenlines/imconnector/imconnector-register.md) — register the connector in Bitrix24,

    - [event.bind](../../api-reference/events/event-bind.md) — subscribe to the event [OnImConnectorMessageAdd](../../api-reference/imopenlines/imconnector/events/on-im-connector-message-add.md).

3. Create a handler `handler.php` for events from Bitrix24. In the file, we will call the methods:

    - [imconnector.activate](../../api-reference/imopenlines/imconnector/imconnector-activate.md) — activate the connector,

    - [imconnector.connector.data.set](../../api-reference/imopenlines/imconnector/imconnector-connector-data-set.md) — pass widget data,

    - [imconnector.send.status.delivery](../../api-reference/imopenlines/imconnector/imconnector-send-status-delivery.md) — confirm message delivery.

4. Create a file `ajax.php` for data exchange between the widget and Bitrix24. In the file, we will call the method [imconnector.send.messages](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md) to send messages.

5. Create a public page `index.php` with the chat interface.

6. Run the script `install_connector.php` to register the connector in the system.

As a result, we will have a chat that is integrated with Bitrix24 and ready to receive messages from users.

The dialogue is tied to:

- the user's session — each visitor receives an independent conversation,

- the site's domain — message exchange works only at the specified address.

## 1\. Create the file function.php

Create the file `function.php` and define helper functions.

{% note info "" %}

In the example, the chat history is stored in files. In real projects, it is recommended to use a database.

{% endnote %}

### getConnectorID

The function `getConnectorID` returns a unique identifier for the connector `example_connector_1`.

{% include [Example Note](../../_includes/examples.md) %}

```php
include_once('crest.php');
function getConnectorID()
{
    return 'example_connector_1';
}
```

### getChat

The function `getChat` returns the chat history `$chatID` from a file. If the file exists, it returns an array of messages `$result`. If not, it returns an empty array.

It takes the parameter `$chatID` — the chat identifier.

```php
function getChat($chatID)
{
    $result = [];
    if (file_exists(__DIR__ . '/chats/' . $chatID . '.txt'))
    {
        $result = json_decode(file_get_contents(__DIR__ . '/chats/' . $chatID . '.txt'), 1);
    }
    return $result;
}
```

### saveMessage

The function `saveMessage` saves a new message `$arMessage` to the chat file `$chatID`.

Parameters:

- `$chatID` — the chat identifier,

- `$arMessage` — an array with message data.

The function returns the message number if it was saved, or `false` in case of a write error.

```php
function saveMessage($chatID, $arMessage)
{
    $arMessages = getChat($chatID);
    $count = count($arMessages);
    $arMessages['message' . $count] = $arMessage;
    if (file_put_contents(__DIR__ . '/chats/' . $chatID . '.txt', json_encode($arMessages)))
    {
        $return = $count;
    }
    else
    {
        $return = false;
    }
    return $return;
}
```

### getLine

The function `getLine` returns the line identifier from the file `line_id.txt`, if the file exists. If the file does not exist, it returns `false` or an empty string.

```php
function getLine()
{
    return file_get_contents(__DIR__ . '/line_id.txt');
}
```

### setLine

The function `setLine` saves the line identifier to the file `line_id.txt`. It takes the parameter `$line_id` — the open line identifier. In case of successful writing, the function returns the number of bytes written, otherwise — `false`.

```php
function setLine($line_id)
{
    return file_put_contents(__DIR__ . '/line_id.txt', intVal($line_id));
}
```

### convertBB

The function `convertBB` converts BB codes to HTML. It takes the parameter `$var` — text with tags like `[b]bold[/b]`. It returns text with HTML tags, for example, `<strong>bold</strong>`.

```php
function convertBB($var)
{
    $search = array(
        '/\[b\](.*?)\[\/b\]/is',
        '/\[br\]/is',
        '/\[i\](.*?)\[\/i\]/is',
        '/\[u\](.*?)\[\/u\]/is',
        '/\[img\](.*?)\[\/img\]/is',
        '/\[url\](.*?)\[\/url\]/is',
        '/\[url\=(.*?)\](.*?)\[\/url\]/is'
    );
    $replace = array(
        '<strong>$1</strong>',
        '<br>',
        '<em>$1</em>',
        '<u>$1</u>',
        '<img src="$1" />',
        '<a href="$1">$1</a>',
        '<a href="$1">$2</a>'
    );
    $var = preg_replace($search, $replace, $var);
    return $var;
}
```

### Example code for the file function.php

```php
<?php
include_once('crest.php');
function getConnectorID()
{
    return 'example_connector_1';
}
function getChat($chatID)
{
    $result = [];
    if (file_exists(__DIR__ . '/chats/' . $chatID . '.txt'))
    {
        $result = json_decode(file_get_contents(__DIR__ . '/chats/' . $chatID . '.txt'), 1);
    }
    return $result;
}
function saveMessage($chatID, $arMessage)
{
    $arMessages = getChat($chatID);
    $count = count($arMessages);
    $arMessages['message' . $count] = $arMessage;
    if (file_put_contents(__DIR__ . '/chats/' . $chatID . '.txt', json_encode($arMessages)))
    {
        $return = $count;
    }
    else
    {
        $return = false;
    }
    return $return;
}
function getLine()
{
    return file_get_contents(__DIR__ . '/line_id.txt');
}
function setLine($line_id)
{
    return file_put_contents(__DIR__ . '/line_id.txt', intVal($line_id));
}
function convertBB($var)
{
    $search = array(
        '/\[b\](.*?)\[\/b\]/is',
        '/\[br\]/is',
        '/\[i\](.*?)\[\/i\]/is',
        '/\[u\](.*?)\[\/u\]/is',
        '/\[img\](.*?)\[\/img\]/is',
        '/\[url\](.*?)\[\/url\]/is',
        '/\[url\=(.*?)\](.*?)\[\/url\]/is'
    );
    $replace = array(
        '<strong>$1</strong>',
        '<br>',
        '<em>$1</em>',
        '<u>$1</u>',
        '<img src="$1" />',
        '<a href="$1">$1</a>',
        '<a href="$1">$2</a>'
    );
    $var = preg_replace($search, $replace, $var);
    return $var;
}
```

## 2\. Create the file install_connector.php

To register the connector in Bitrix24, create the file `install_connector.php`. In the file, include `function.php`, which we created in the first step.

```php
require_once('function.php');
```

### Set up the event handler URL

In the parameter `$handlerUrl`, specify the address of the script that will receive events from Bitrix24. We will create the file with the script `handler.php` in the third step.

{% note warning "" %}

The address must be valid, accessible from the outside, and support HTTPS.

{% endnote %}

```php
$handlerUrl = 'https://yourdomain.com/handler.php';
```

### Create a directory to store chats

Create a folder `/chats/` to store the history of conversations.

1. Use `@mkdir()` to create the directory with permissions `0775`.

2. The flag `true` allows creating nested folders.

3. Check if the folder was created successfully.

```php
@mkdir(__DIR__ . '/chats/', 0775, true);
if(!file_exists(__DIR__ . '/chats/'))
{
    echo 'error create dir "chats"';
}
```

### Register the connector

Get the connector identifier `$connector_id` through the function `getConnectorID()` from `function.php`.

To register the connector, use the method [imconnector.register](../../api-reference/imopenlines/imconnector/imconnector-register.md). We will pass the following data to it:

- `ID` — the connector identifier `$connector_id`.

- `NAME` — the name of the connector. We will specify `ExampleSiteChat`.

- `ICON` — an array for describing the connector's icon in an active state.

    - `DATA_IMAGE` — DATA representation of the SVG icon. For example, `data:image/svg+xml;charset=US-ASCII,…`.

    - `COLOR` — color. We will specify `#a6ffa3`.

    - `SIZE` — size. We will pass `100%`.

    - `POSITION` — position of the SVG. We will set it to `center`.

- `ICON_DISABLED` — an array for describing the icon when the connector is disabled. It is similar to the `ICON` array.

- `PLACEMENT_HANDLER` — the URL of the event handler. We will pass `$handlerUrl`.

```php
$connector_id = getConnectorID();
$result = CRest::call(
    'imconnector.register',
    [
        'ID' => $connector_id,
        'NAME' => 'ExampleSiteChat',
        'ICON' => [
            'DATA_IMAGE' => 'data:image/svg+xml;charset=US-ASCII,...',
            'COLOR' => '#a6ffa3',
            'SIZE' => '100%',
            'POSITION' => 'center',
        ],
        'ICON_DISABLED' => [
            'DATA_IMAGE' => 'data:image/svg+xml;charset=US-ASCII,...',
            'SIZE' => '100%',
            'POSITION' => 'center',
            'COLOR' => '#ffb3a3',
        ],
        'PLACEMENT_HANDLER' => $handlerUrl,
    ]
);
```

### Register the event handler

After successfully registering the connector, we will subscribe to the event [OnImConnectorMessageAdd](../../api-reference/imopenlines/imconnector/events/on-im-connector-message-add.md). It is triggered when a new message is received from the user.

To register the event handler, use the method [event.bind](../../api-reference/events/event-bind.md). We will pass the following parameters to it:

- `event` — the name of the event. We will pass `OnImConnectorMessageAdd`.

- `handler` — a link to the event handler. We will specify `$handlerUrl`.

After registration, we will output the message `successfully`.

```php
if (!empty($result['result']))
{
    $resultEvent = CRest::call(
        'event.bind',
        [
            'event' => 'OnImConnectorMessageAdd',
            'handler' => $handlerUrl,
        ]
    );
    if (!empty($resultEvent['result']))
    {
        echo 'successfully';
    }
}
```

### Example code for the file install_connector.php

```php
<?php
require_once('function.php');
$handlerUrl = 'https://yourdomain.com/handler.php';
//create dir for save chats (recommend using database)
@mkdir(__DIR__ . '/chats/', 0775, true);
if(!file_exists(__DIR__ . '/chats/'))
{
    echo 'error create dir "chats"';
}
else
{
    $connector_id = getConnectorID();
    $result = CRest::call(
        'imconnector.register',
        [
            'ID' => $connector_id,
            'NAME' => 'ExampleSiteChat',
            'ICON' => [
                'DATA_IMAGE' => 'data:image/svg+xml;charset=US-ASCII,%3Csvg%20version%3D%221.1%22%20id%3D%22Layer_1%22%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20x%3D%220px%22%20y%3D%220px%22%0A%09%20viewBox%3D%220%200%2070%2071%22%20style%3D%22enable-background%3Anew%200%200%2070%2071%3B%22%20xml%3Aspace%3D%22preserve%22%3E%0A%3Cpath%20fill%3D%22%230C99BA%22%20class%3D%22st0%22%20d%3D%22M34.7%2C64c-11.6%2C0-22-7.1-26.3-17.8C4%2C35.4%2C6.4%2C23%2C14.5%2C14.7c8.1-8.2%2C20.4-10.7%2C31-6.2%0A%09c12.5%2C5.4%2C19.6%2C18.8%2C17%2C32.2C60%2C54%2C48.3%2C63.8%2C34.7%2C64L34.7%2C64z%20M27.8%2C29c0.8-0.9%2C0.8-2.3%2C0-3.2l-1-1.2h19.3c1-0.1%2C1.7-0.9%2C1.7-1.8%0A%09v-0.9c0-1-0.7-1.8-1.7-1.8H26.8l1.1-1.2c0.8-0.9%2C0.8-2.3%2C0-3.2c-0.4-0.4-0.9-0.7-1.5-0.7s-1.1%2C0.2-1.5%2C0.7l-4.6%2C5.1%0A%09c-0.8%2C0.9-0.8%2C2.3%2C0%2C3.2l4.6%2C5.1c0.4%2C0.4%2C0.9%2C0.7%2C1.5%2C0.7C26.9%2C29.6%2C27.4%2C29.4%2C27.8%2C29L27.8%2C29z%20M44%2C41c-0.5-0.6-1.3-0.8-2-0.6%0A%09c-0.7%2C0.2-1.3%2C0.9-1.5%2C1.6c-0.2%2C0.8%2C0%2C1.6%2C0.5%2C2.2l1%2C1.2H22.8c-1%2C0.1-1.7%2C0.9-1.7%2C1.8v0.9c0%2C1%2C0.7%2C1.8%2C1.7%2C1.8h19.3l-1%2C1.2%0A%09c-0.5%2C0.6-0.7%2C1.4-0.5%2C2.2c0.2%2C0.8%2C0.7%2C1.4%2C1.5%2C1.6c0.7%2C0.2%2C1.5%2C0%2C2-0.6l4.6-5.1c0.8-0.9%2C0.8-2.3%2C0-3.2L44%2C41z%20M23.5%2C32.8%0A%09c-1%2C0.1-1.7%2C0.9-1.7%2C1.8v0.9c0%2C1%2C0.7%2C1.8%2C1.7%2C1.8h23.4c1-0.1%2C1.7-0.9%2C1.7-1.8v-0.9c0-1-0.7-1.8-1.7-1.9L23.5%2C32.8L23.5%2C32.8z%22/%3E%0A%3C/svg%3E%0A',
                'COLOR' => '#a6ffa3',
                'SIZE' => '100%',
                'POSITION' => 'center',
            ],
            'ICON_DISABLED' => [
                'DATA_IMAGE' => 'data:image/svg+xml;charset=US-ASCII,%3Csvg%20version%3D%221.1%22%20id%3D%22Layer_1%22%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20x%3D%220px%22%20y%3D%220px%22%0A%09%20viewBox%3D%220%200%2070%2071%22%20style%3D%22enable-background%3Anew%200%200%2070%2071%3B%22%20xml%3Aspace%3D%22preserve%22%3E%0A%3Cpath%20fill%3D%22%230C99BA%22%20class%3D%22st0%22%20d%3D%22M34.7%2C64c-11.6%2C0-22-7.1-26.3-17.8C4%2C35.4%2C6.4%2C23%2C14.5%2C14.7c8.1-8.2%2C20.4-10.7%2C31-6.2%0A%09c12.5%2C5.4%2C19.6%2C18.8%2C17%2C32.2C60%2C54%2C48.3%2C63.8%2C34.7%2C64L34.7%2C64z%20M27.8%2C29c0.8-0.9%2C0.8-2.3%2C0-3.2l-1-1.2h19.3c1-0.1%2C1.7-0.9%2C1.7-1.8%0A%09v-0.9c0-1-0.7-1.8-1.7-1.8H26.8l1.1-1.2c0.8-0.9%2C0.8-2.3%2C0-3.2c-0.4-0.4-0.9-0.7-1.5-0.7s-1.1%2C0.2-1.5%2C0.7l-4.6%2C5.1%0A%09c-0.8%2C0.9-0.8%2C2.3%2C0%2C3.2l4.6%2C5.1c0.4%2C0.4%2C0.9%2C0.7%2C1.5%2C0.7C26.9%2C29.6%2C27.4%2C29.4%2C27.8%2C29L27.8%2C29z%20M44%2C41c-0.5-0.6-1.3-0.8-2-0.6%0A%09c-0.7%2C0.2-1.3%2C0.9-1.5%2C1.6c-0.2%2C0.8%2C0%2C1.6%2C0.5%2C2.2l1%2C1.2H22.8c-1%2C0.1-1.7%2C0.9-1.7%2C1.8v0.9c0%2C1%2C0.7%2C1.8%2C1.7%2C1.8h19.3l-1%2C1.2%0A%09c-0.5%2C0.6-0.7%2C1.4-0.5%2C2.2c0.2%2C0.8%2C0.7%2C1.4%2C1.5%2C1.6c0.7%2C0.2%2C1.5%2C0%2C2-0.6l4.6-5.1c0.8-0.9%2C0.8-2.3%2C0-3.2L44%2C41z%20M23.5%2C32.8%0A%09c-1%2C0.1-1.7%2C0.9-1.7%2C1.8v0.9c0%2C1%2C0.7%2C1.8%2C1.7%2C1.8h23.4c1-0.1%2C1.7-0.9%2C1.7-1.8v-0.9c0-1-0.7-1.8-1.7-1.9L23.5%2C32.8L23.5%2C32.8z%22/%3E%0A%3C/svg%3E%0A',
                'SIZE' => '100%',
                'POSITION' => 'center',
                'COLOR' => '#ffb3a3',
            ],
            'PLACEMENT_HANDLER' => $handlerUrl,
        ]
    );
    if (!empty($result['result']))
    {
        $resultEvent = CRest::call(
            'event.bind',
            [
                'event' => 'OnImConnectorMessageAdd',
                'handler' => $handlerUrl,
            ]
        );
        if (!empty($resultEvent['result']))
        {
            echo 'successfully';
        }
    }
}
```

## 3\. Create the file handler.php

Create the file `handler.php` to process events from Bitrix24. In the file, include `function.php`, which we created in the first step.

```php
require_once('function.php');
```

### Set up widget parameters

- `$widgetUri` — specify the URL of the page with the widget icon.

- `$widgetName` — set the name of the connector in the widget.

- `$connector_id` — pass the connector identifier through the function `getConnectorID` from the file `function.php`.

```php
$widgetUri = '';
$widgetName = 'ExampleSiteChatWidget';
$connector_id = getConnectorID();
```

The variables `$widgetUri` and `$widgetName` are required if you need to show the connector in the list of connectors in the widget on the site. In other cases, they can be left empty.

### Activate the connector

In the array `$options`, pass the processed data from `PLACEMENT_OPTIONS`. The array contains the line identifier and the connector status.

To activate the connector, use the method [imconnector.activate](../../api-reference/imopenlines/imconnector/imconnector-activate.md). We will pass the following data to it:

- `CONNECTOR` — the connector identifier `$connector_id`.

- `LINE` — the open line identifier. We will specify `intVal($options['LINE'])`.

- `ACTIVE` — the connector status. We will pass `intVal($options['ACTIVE_STATUS'])`. Possible values: `1` — active, `0` — inactive.

```php
$options = json_decode($_REQUEST['PLACEMENT_OPTIONS'], true);
$result = CRest::call(
    'imconnector.activate',
    [
        'CONNECTOR' => $connector_id,
        'LINE' => intVal($options['LINE']),
        'ACTIVE' => intVal($options['ACTIVE_STATUS']),
    ]
);
```

### Supplement the connector settings

If the connector is activated and `widgetUri` and `widgetName` are specified, we will supplement the connector settings using the method [imconnector.connector.data.set](../../api-reference/imopenlines/imconnector/imconnector-connector-data-set.md). We will pass the following data to it:

- `CONNECTOR` — the connector identifier `$connector_id`.

- `LINE` — the open line identifier. We will specify `intVal($options['LINE'])`.

- `DATA` — an array with data to save.

    - `id` — the identifier of the account connected to this connector. We will specify `$connector_id.'line'.intVal($options['LINE'])` — a combination of the connector and line identifiers.

    - `url_im` — the link to the chat. We will pass the parameter `$widgetUri`.

    - `name` — the name of the connector in the widget. We will pass the parameter `$widgetName`.

```php
$resultWidgetData = CRest::call(
    'imconnector.connector.data.set',
    [
        'CONNECTOR' => $connector_id,
        'LINE' => intVal($options['LINE']),
        'DATA' => [
            'id' => $connector_id.'line'.intVal($options['LINE']),
            'url_im' => $widgetUri,
            'name' => $widgetName
        ],
    ]
);
```

### Save the open line

After successful activation:

- save the line identifier to a file using the function `setLine` from the file `function.php`,

- output the message `successfully`.

```php
if(!empty($resultWidgetData['result']))
{
    setLine($options['LINE']);
    echo 'successfully';
}
```

### Confirm message delivery

When the event `ONIMCONNECTORMESSAGEADD` is triggered, and a message is received for the connector with the identifier `$connector_id`, save the message using the function `saveMessage`.

Confirm message delivery using the method [imconnector.send.status.delivery](../../api-reference/imopenlines/imconnector/imconnector-send-status-delivery.md). We will pass the following data to it:

- `CONNECTOR` — the connector identifier `$connector_id`.

- `LINE` — the open line identifier. We will specify it using the function `getLine` from the file `function.php`.

- `MESSAGES` — an array of messages.

    - `im` — the element from the incoming message of the open line.

    - `message` — an array of message identifiers. We will pass `$idMess`, which is obtained from the function `saveMessage`.

    - `chat` — the chat identifier.

```php
if(
    $_REQUEST['event'] == 'ONIMCONNECTORMESSAGEADD'
    && !empty($_REQUEST['data']['CONNECTOR'])
    && $_REQUEST['data']['CONNECTOR'] == $connector_id
    && !empty($_REQUEST['data']['MESSAGES'])
)
{
    foreach ($_REQUEST['data']['MESSAGES'] as $arMessage)
    {
        $idMess = saveMessage($arMessage['chat']['id'], $arMessage);
        $resultDelivery = CRest::call(
            'imconnector.send.status.delivery',
            [
                'CONNECTOR' => $connector_id,
                'LINE' => getLine(),
                'MESSAGES' => [
                    [
                        'im' => $arMessage['im'],
                        'message' => [
                            'id' => [$idMess]
                        ],
                        'chat' => [
                            'id' => $arMessage['chat']['id']
                        },
                    ],
                ]
            ]
        );
    }
}
```

### Example code for the file handler.php

```php
<?php
require_once('function.php');
$widgetUri = '';
$widgetName = 'ExampleSiteChatWidget';
$connector_id = getConnectorID();
if (!empty($_REQUEST['PLACEMENT_OPTIONS']) && $_REQUEST['PLACEMENT'] == 'SETTING_CONNECTOR')
{
    //activate connector
    $options = json_decode($_REQUEST['PLACEMENT_OPTIONS'], true);
    $result = CRest::call(
        'imconnector.activate',
        [
            'CONNECTOR' => $connector_id,
            'LINE' => intVal($options['LINE']),
            'ACTIVE' => intVal($options['ACTIVE_STATUS']),
        ]
    );
    if (!empty($result['result']))
    {
        //add data widget
        if(!empty($widgetUri) && !empty($widgetName))
        {
            $resultWidgetData = CRest::call(
                'imconnector.connector.data.set',
                [
                    'CONNECTOR' => $connector_id,
                    'LINE' => intVal($options['LINE']),
                    'DATA' => [
                        'id' => $connector_id.'line'.intVal($options['LINE']),
                        'url_im' => $widgetUri,
                        'name' => $widgetName
                    ],
                ]
            );
            if(!empty($resultWidgetData['result']))
            {
                setLine($options['LINE']);
                echo 'successfully';
            }
        }
        else
        {
            setLine($options['LINE']);
            echo 'successfully';
        }
    }
}
if(
    $_REQUEST['event'] == 'ONIMCONNECTORMESSAGEADD'
    && !empty($_REQUEST['data']['CONNECTOR'])
    && $_REQUEST['data']['CONNECTOR'] == $connector_id
    && !empty($_REQUEST['data']['MESSAGES'])
)
{
    foreach ($_REQUEST['data']['MESSAGES'] as $arMessage)
    {
        $idMess = saveMessage($arMessage['chat']['id'], $arMessage);
        $resultDelivery = CRest::call(
            'imconnector.send.status.delivery',
            [
                'CONNECTOR' => $connector_id,
                'LINE' => getLine(),
                'MESSAGES' => [
                    [
                        'im' => $arMessage['im'],
                        'message' => [
                            'id' => [$idMess]
                        ],
                        'chat' => [
                            'id' => $arMessage['chat']['id']
                        },
                    ],
                ]
            ]
        );
    }
}
```

## 4\. Create the file ajax.php

Create the file `ajax.php` to handle AJAX requests from the chat widget on the site. In the file, include `function.php`, which contains the necessary functions.

```php
require_once('function.php');
session_start();
```

### Prepare variables

- `$chatID` — the chat identifier. Create it based on the domain `HTTP_ORIGIN` and the user session `session_id()`.

- `$type` — the request type. Pass the value from the form `$_POST['type']`. Possible values: `chat_history` — load history, `send_message` — send a message.

- `$connector_id` — the connector identifier. Obtain it using the function `getConnectorID` from `function.php`.

- `$line_id` — the open line identifier. Obtain it using the function `getLine` from `function.php`.

```php
$chatID = 'chat' . md5($_SERVER['HTTP_ORIGIN']) . md5(session_id());
$type = $_POST['type'];
$connector_id = getConnectorID();
$line_id = getLine();
```

### Load chat history

For an AJAX request of type `chat_history`, load messages using the function `getChat` from `function.php`.

1. Output each message as a block.

    - If the element `im` exists, the message came from Bitrix24. Display it on the left with a gray background `#fbfbfb`.

    - If the element `im` does not exist — the message is from the user. Display it on the right with a light blue background `#ccf2ff`.

2. Process the text using the function `convertBB()` from `function.php`.

```php
if ($type == 'chat_history'):
    $arChat = getChat($chatID);
    if (!empty($arChat)):
        foreach ($arChat as $item): ?>
            <div class="col-12 alert alert-warning text-<?=(!empty($item['im'])) ? 'left' : 'right'?>"
                style=" background-color: <?=(!empty($item['im'])) ? '#fbfbfb' : '#ccf2ff'?>">
                <?=convertBB($item['message']['text'])?>
            </div>
        <?php endforeach;
    endif;
```

### Send a message

For an AJAX request of type `send_message`, form the message structure `$arMessage` and send it to Bitrix24.

#### Structure of the array \$arMessage

- `user` — an array describing the user:

    - `id` — the user identifier, which matches the chat identifier. Pass `$chatID`.

    - `name` — the user's name. Protect against XSS attacks using `htmlspecialchars` and pass the value from the form `$_POST['name']`.

- `message` — an array describing the message:

    - `date` — the message time in `timestamp` format.

    - `text` — the message text. Protect against XSS attacks using `htmlspecialchars` and pass the value from the form `$_POST['message']`.

- `chat` — an array describing the chat:

    - `id` — the chat identifier. Pass `$chatID`.

    - `url` — the link to the chat in the external system. Protect against XSS attacks using `htmlspecialchars` and pass the address of the page `HTTP_REFERER`.

```php
elseif ($type == 'send_message'):
    $arMessage = [
        'user' => [
            'id' => $chatID,
            'name' => htmlspecialchars($_POST['name']),
        ],
        'message' => [
            'id' => false,
            'date' => time(),
            'text' => htmlspecialchars($_POST['message']),
        ],
        'chat' => [
            'id' => $chatID,
            'url' => htmlspecialchars($_SERVER['HTTP_REFERER']),
        ],
    ];
```

#### Save the message locally

Add the message to the file using the function `saveMessage`. Save the message number in the variable `$id`.

```php
$id = saveMessage($chatID, $arMessage);
```

#### Pass the message to Bitrix24

To send the message to the open line, use the method [imconnector.send.messages](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md). We will pass the following data to it:

- `CONNECTOR` — the connector identifier `$connector_id`.

- `LINE` — the open line identifier. We will specify `$line_id`, which is obtained using `getLine`.

- `MESSAGES` — an array of messages. Each message is described by a separate array. We will pass `[$arMessage]`.

```php
if ($id !== false)
{
    $arMessage['message']['id'] = $id;
    $result = CRest::call(
        'imconnector.send.messages',
        [
            'CONNECTOR' => $connector_id,
            'LINE' => $line_id,
            'MESSAGES' => [$arMessage],
        ]
    );
}
echo json_encode(
    [
        'chat' => $chatID,
        'post' => $_POST,
        'result' => $result
    ]
);
```

### Example code for the file ajax.php

```php
<?php
require_once('function.php');
session_start();
$chatID = 'chat' . md5($_SERVER['HTTP_ORIGIN']) . md5(session_id());
$type = $_POST['type'];
$connector_id = getConnectorID();
$line_id = getLine();
/*
    simple example save chat, must lost any data
    recommend using database
*/
if ($type == 'chat_history'):
    $arChat = getChat($chatID);
    if (!empty($arChat)):
        foreach ($arChat as $item): ?>
            <div class="col-12 alert alert-warning text-<?=(!empty($item['im'])) ? 'left' : 'right'?>"
                style=" background-color: <?=(!empty($item['im'])) ? '#fbfbfb' : '#ccf2ff'?>">
                <?=convertBB($item['message']['text'])?>
            </div>
        <?php endforeach;
    endif;
elseif ($type == 'send_message'):
    $arMessage = [
        'user' => [
            'id' => $chatID,
            'name' => htmlspecialchars($_POST['name']),
        ],
        'message' => [
            'id' => false,
            'date' => time(),
            'text' => htmlspecialchars($_POST['message']),
        ],
        'chat' => [
            'id' => $chatID,
            'url' => htmlspecialchars($_SERVER['HTTP_REFERER']),
        ],
    ];
    $id = saveMessage($chatID, $arMessage);
    $result['error'] = 'error_save';
    if ($id !== false)
    {
        $arMessage['message']['id'] = $id;
        $result = CRest::call(
            'imconnector.send.messages',
            [
                'CONNECTOR' => $connector_id,
                'LINE' => $line_id,
                'MESSAGES' => [$arMessage],
            ]
        );
    }
    echo json_encode(
        [
            'chat' => $chatID,
            'post' => $_POST,
            'result' => $result
        ]
    );
endif;
```

## 5\. Create the file index.php

Create a public chat page for visitors — the file `index.php`.

1. Include external libraries Bootstrap and jQuery.

2. Create a chat container consisting of two parts:

    - `chat_history` — message history,

    - `chat_form` — message sending form.

3. Implement the chat logic through JavaScript.

    - Add the function `updateChat`, which updates the message history every five seconds. It sends a POST request to `ajax.php` with the parameter `type=chat_history`, receives the HTML markup of the messages, and inserts it into `#chat_history`.

    - Handle message sending. Collect data using the `serialize` function and perform a request to `ajax.php` with the parameter `type=send_message`.

### Example code for the file index.php

```javascript
<body>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css"
         crossorigin="anonymous">
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <div class="container-fluid">
        <div class="m-5">
            <div id="chat_history" class="row">
                <div class="spinner-border m-5 text-success" role="status">
                    <span class="sr-only">Loading...</span>
                </div>
            </div>
            <div id="chat_form" class="mt-5 mr-auto ml-auto mb-5">
                <form id="form_message">
                    <div class="form-group">
                        <label for="name">Name</label>
                        <input type="text" class="form-control" placeholder="Name">
                    </div>
                    <div class="form-group">
                        <label for="message">Message</label>
                        <textarea class="form-control" name="message" rows="3" placeholder="your message here"></textarea>
                    </div>
                    <input class="btn btn-primary" type="submit" name="send" value="send">
                </form>
            </div>
        </div>
    </div>
    <script>
        $(document).ready(function () {
            function updateChat()
            {
                $.ajax({
                    'method': 'POST',
                    'dataType': 'html',
                    'url': 'ajax.php',
                    'data': 'type=chat_history',
                    success: function (data) {//success callback
                        $('#chat_history').text('').html(data);
                    }
                });
            }
            setInterval(updateChat, 5000);
            updateChat();
            $('#form_message').on('submit', function (el) {//event submit form
                el.preventDefault();//the default action of the event will not be triggered
                $('#chat_form').addClass('spinner-border');
                $('#form_message').hide();
                var formData = $(this).serialize();
                $.ajax({
                    'method': 'POST',
                    'dataType': 'json',
                    'url': 'ajax.php',
                    'data': formData + '&type=send_message',
                    success: function (data) {//success callback
                        updateChat();
                        $('#chat_form').removeClass('spinner-border');
                        $('#form_message textarea[name=message]').val('');
                        $('#form_message').show();
                    }
                });
            });
        });
    </script>
</body>
```

## 6\. Run the connector

1. Place the files `function.php`, `handler.php`, `install_connector.php`, `ajax.php`, `index.php` in one folder on the server. For example, in the folder `/myconnector/`.

2. In the browser, open the page `https://your_site.com/myconnector/install_connector.php` and see one of two messages:

    - `successfully` — the folder `/chats/` was created, the connector was registered,

    - `error create dir "chats"` — the script cannot create the folder `/chats/`, the connector setup was not completed. To fix this, you need to correctly configure the permissions.

3. Connect the connector in Bitrix24.

    - In Bitrix24, go to the page *CRM > Clients > Contact Center*.

    - Find the block named ExampleSiteChat.

    - Click Connect.

    - If the message `successfully` appears, the connector is activated.

4. Start a dialogue in the chat.

    - In the browser, open the page `https://your_site.com/myconnector/index.php`.

    - Write a message. It should arrive in Bitrix24.

    - Reply — the response will appear on the site.