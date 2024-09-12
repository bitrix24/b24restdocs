# Example of Creating a Support Channel

With the **Open Channels** module, you can set up a technical support line for any *Bitrix24* application, including chatbots.

The following procedure is required:

- Go to the **Contact Center** section and connect the communication channel `Bitrix24.Network`:

    ![Adding Bitrix24.Network](./_images/add_network01.png)

- After that, the connection settings form for `Bitrix24.Network` will be available:

    ![Bitrix24.Network Settings](./_images/add_network02.png)

- Be sure to fill in the `Name` and `Brief description` fields, and set an `Avatar` — this will help clients find you more easily.

- Once a user enters your Open Channel *Bitrix24.Network*, they will automatically receive a welcome message:

    ![Welcome Message](./_images/openlines4.png)

- Create a new open technical support line for your product or select an existing one:

    ![Creating or Selecting an Open Channel](./_images/add_network000.png)

Your open channel can be auto connected to the user's account using the [imopenlines.network.join](../../api-reference/imopenlines/openlines/imopenlines-network-join.md) REST command:

```php
$result = restCommand(
    'imopenlines.network.join',
    Array(
        'CODE' => 'a588e1a88baaf301b9d0b0b33b1eefc2b' // search code as specified on the Connectors page
    ),
    $_REQUEST[
        "auth"
    ]
);
```

After the **Open Channel** is configured, you can write a welcome message to a client via the [imopenlines.network.message.add](../../api-reference/imopenlines/openlines/imopenlines-network-message-add.md) REST command:

```text
Thank you for installation, we will be glad to help, if you have any questions - write to this chat. Have a good Day! :)
```

{% note info %}

The `restCommand` unction is used here for illustration only. You can send a REST command with your own function, or use the Javascript-[BX24.callMethod](../../how-to-use-examples.md), or [bitrix24-php-sdk](https://github.com/mesilov/bitrix24-php-sdk) methods. It is also possible to open such support channel via  [BX24.im.openMessenger](../../api-reference/bx24-js-sdk/additional-functions/bx24-im-open-messenger.md) JavaScript-method.

{% endnote %}