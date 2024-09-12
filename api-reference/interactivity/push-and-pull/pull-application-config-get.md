# Organize Instant Communications within Applications pull.application.config.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`pull`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `pull.application.config.get` is used to retrieve information about the connection to real-time servers and to organize instant communications within applications.

By connecting to RT servers, you can:
- create a truly interactive application,
- change states,
- instantly update the interface without the need to refresh the page in real-time.

{% note warning %}

The method will return data about the connection to channels specifically created for your REST application. Within these channels, you will receive only your events.

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **CACHE** | `Y / N` Whether to return cached data or not, default is Y. ||
|#

## Examples

{% list tabs %}

- JavaScript

    ```js
    BX24.callMethod('pull.application.config.get', {
        'CACHE': 'Y',
    }, function(result){
        if(result.error())
        {
            console.error(result.error().ex);
        }
        else
        {
            console.log(result.data());
        }
    });
    ```

- PHP
  
    ```php
    $result = restCommand('pull.application.config.get', [
        'CACHE' => 'Y',
    ], $_REQUEST["auth"]);
    ```

{% endlist %}

{% include [Examples Note](../../../_includes/examples.md) %}

## Successful Response

> 200 OK

```json
{
    "result": {
        "server": {
            "version": 4,
            "server_enabled": true,
            "long_polling": "http://rt.bitrix24.com/sub/",
            "long_polling_secure": "https://rt.bitrix24.com/sub/",
            "websocket_enabled": true,
            "websocket": "ws://rt.bitrix24.com/sub/",
            "websocket_secure": "wss://rt.bitrix24.com/sub/",
            "publish_enabled": true,
            "publish": "http://rt.bitrix24.com/pubweb/",
            "publish_secure": "https://rt.bitrix24.com/pubweb/"
        },
        "channels": {
            "shared": {
                "id": "46a437d2336d4a88e4e9b3cd956ecf45.7910bb25e660bf211fdec15e33c5e25e4c3b644a",
                "start": "2017-06-28T12:04:00+02:00",
                "end": "2017-06-29T00:04:00+02:00",
                "type": "shared"
            },
            "private": {
                "id": "925153cd80b6b5a4dbf8659d5be21d1:abe9e6964532000ab8b7acf092ba627b.605ea91793ad24be3f9745d662713b23a5803a94",
                "public_id": "abe9e6964532000ab8b7acf092ba627b.057ac8625ae4ac0da4ed093a19950f9dab7e29d0",
                "start": "2017-06-28T09:57:48+02:00",
                "end": "2017-06-28T21:57:48+02:00",
                "type": "private"
            }
        }
    }
}
```

The **server** object describes the server configuration and paths for connecting to the real-time channel. The keys of the object:

- **version** - version of the installed server,
- **server_enabled** - whether the server is activated,
- **websocket_enabled** - whether websocket functionality is available,
- **long_polling** and **websocket** - connection paths,
- **long_polling_secure** and **websocket_secure** - connection paths when using the HTTPS protocol,
- **publish_enabled** - whether the [message publishing capability](*key_capability) is available from the client side. 
- **publish** and **publish_secure** - paths for publishing messages from the client side,
- **clientId** - unique identifier of the account on the cloud push server. Returned if the account uses a cloud push server.

The **channels** object describes the data for connecting the user to the channels. The keys:

- **shared** - the shared channel of the account. Commands for all users of the account (including extranet users) are published on this channel.
- **private** - the user's private channel. Commands are published only for the current user on this channel.

The channel array contains:

- **id** - channel identifier;
- **public_id** - public [channel identifier](*key_identifier);
- **start** - time of channel creation (in ATOM format);
- **end** - time of channel operation (in ATOM format);
- **type** - type of channel.
  
## Error Response

> 200 Error, 50x Error

```json
{
    "error": "SERVER_ERROR",
    "error_description": "Push & Pull server is not configured"
}
```

Keys:

- **error** - code of the occurred error
- **error_description** - brief description of the occurred error
  
### Possible Error Codes

#|
|| **Code** | **Description** ||
|| SERVER_ERROR | The **Push & Pull** module is not configured to work with the queue server on the account. ||
|| WRONG_AUTH_TYPE | The method can only be used within [OAuth 2.0](https://training.bitrix24.com/support/training/course/index.php?COURSE_ID=169&LESSON_ID=20110&LESSON_PATH=13643.20052.20096.20110) or via [webhooks](https://training.bitrix24.com/support/training/course/index.php?COURSE_ID=169&LESSON_ID=20176&LESSON_PATH=13643.20142.20176). ||
|#

## See Also

- [Interactivity in Applications](https://training.bitrix24.com/support/training/course/index.php?COURSE_ID=169&CHAPTER_ID=020088&LESSON_PATH=13643.20052.20088)

[*key_capability]: Available starting from version 4 of the queue server.

[*key_identifier]: Available only for version 4 of the queue server and only for private channels.