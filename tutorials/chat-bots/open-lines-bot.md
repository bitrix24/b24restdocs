# Example of Creating a Chatbot for Open Lines

The process of creating a chatbot for *Open Lines* is similar to [creating a regular chatbot](./index.md), but there are two differences:

1. When creating a chatbot for *Open Lines*, in [imbot.register](../../api-reference/chat-bots/imbot-register.md), the `TYPE` parameter must be set to `O`.

2. If you need to extend the capabilities of an existing chatbot, you should pass the new key `OPENLINE => Y`, and then the chatbot will operate in hybrid mode.

   In hybrid mode, the chatbot must function correctly in group chats, personal chats, and open line chats. To achieve this, you need to check the `CHAT_ENTITY_TYPE` parameter in all incoming events ([ONIMBOTMESSAGEADD](../../api-reference/chat-bots/messages/events/on-imbot-message-add.md) and [ONIMBOTJOINCHAT](../../api-reference/chat-bots/chats/events/on-imbot-join-chat.md)) — for *Open Lines*, it should be `CHAT_ENTITY_TYPE => LINES`.

In all other respects, it is the familiar and already known [chatbot](./index.md).

For closer integration with *Open Lines*, you need to have access permissions for the scope [`imopenlines`](../../api-reference/scopes/permissions.md).

With such permissions, the following commands will be available:

- [imopenlines.network.join](../../api-reference/imopenlines/openlines/imopenlines-network-join.md) — connecting your company's open line to the *Bitrix24* account, after which employees will be able to message you.
- [imopenlines.bot.session.operator](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-operator.md) — switching the conversation to an available operator.
- [imopenlines.bot.session.transfer](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-transfer.md) — transferring the conversation to a specific operator.
- [imopenlines.bot.session.finish](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-finish.md) — ending the current session.

{% note warning %}

Using an HTTPS certificate for chatbots is not mandatory, but it is highly recommended to protect client confidential data. The application should be encoded in `UTF-8`.

{% endnote %}

## Download the Example Chatbot for Open Lines

As an example of a chatbot for open lines, we have prepared the "ITR Bot." You can obtain it in the following ways:

- [download](https://github.com/bitrix24com/bots) from GitHub (file `itr.php`)
- find and copy it in the *"Bitrix24 on-premise"* product in the folder `\Bitrix\ImBot\Bot\OpenlinesMenuExample`.

This chatbot serves as the first line of support: initially, all messages will go to it, and only then to employees in the queue. The time after which messages will be forwarded from the chatbot to employees is set in the open line settings.

Additionally, a class for building a multi-level menu in chats has been added to the chatbot.

## Running on Your Account

You can [take the example chatbot code](#download-the-example-chatbot-for-open-lines) above, upload it to your server, and run the chatbot on your account as a local application without publishing it through the *Marketplace*:

- In the left menu under **Applications** (1), go to the **Developer resources** (2) subsection and select **Other** (3):

![Adding application](./_images/chatbot1_sm.jpg)

- Use the **Local application** script (4):

![Local application](./_images/chatbot2_sm.jpg)

- Select the application type **Server** and configure its parameters:
  - Change the bot's name.
  - Enable the option `Uses only API` and grant the application access permissions for:
     - `Creating and managing Chatbots` — without these permissions, the application will not be able to register the chatbot.
     - `Open Lines` — without these permissions, the application will not be able to work with open lines.
  - Since the script is written in such a way that it acts as a handler for all events, the URLs in the `Path of your handler` and `Path for initial setup` fields will lead to the same URL.

![Add ITR Bot application](./_images/chatbot3_sm.png)

- After saving the settings, additional fields will appear: `Application code (client_id)` and `Application key (client_secret)`.

![Bot settings](./_images/chatbot4_sm.png)

- Copy the data from these fields and paste it into the `itr.php` file:

![Open lines settings](./_images/chatbot5.png)

- In the bot settings, click `Reinstall`. Now the bot is ready to work.

This bot does not publish messages about being invited to the account. After installation, it will be available in the open lines settings. Select it as responsible and specify the time after which the conversation will be transferred from the chatbot to the queue for employees:

![Open lines settings](./_images/ol_options_sm.png)

{% note info %}

The client can switch to an operator earlier by sending the message `0` or selecting the menu item `0. Wait operator answer`.

With any chatbot, pressing `0` will redirect the user to an operator; no additional processing is required.

{% endnote %}

Below is a dialogue: first, "ITR Bot" responds, the client clicks on the menu items, and then the queue switches to the operator (the client selected the menu item **0. Wait operator answer**):

![Dialogue with ITR Bot](./_images/ol_chat_sm.png)

You can configure your own menu in "ITR Bot" in the `itrRun` method:

```php
/**
* Run ITR menu
*
* @param $portalId
* @param $dialogId
* @param $userId
* @param string $message
* @return bool
*/
function itrRun($portalId, $dialogId, $userId, $message = '')
{
    if ($userId <= 0)
        return false;

    $menu0 = new ItrMenu(0);
    $menu0->setText('Main menu (#0)');
    $menu0->addItem(1, 'Text', ItrItem::sendText('Text message (for #USER_NAME#)'));
    $menu0->addItem(2, 'Text without menu', ItrItem::sendText('Text message without menu', true));
    $menu0->addItem(3, 'Open menu #1', ItrItem::openMenu(1));
    $menu0->addItem(0, 'Wait operator answer', ItrItem::sendText('Wait operator answer', true));

    $menu1 = new ItrMenu(1);
    $menu1->setText('Second menu (#1)');
    $menu1->addItem(2, 'Transfer to queue', ItrItem::transferToQueue('Transfer to queue'));
    $menu1->addItem(3, 'Transfer to user', ItrItem::transferToUser(1, false, 'Transfer to user #1'));
    $menu1->addItem(4, 'Transfer to bot', ItrItem::transferToBot('marta', true, 'Transfer to bot Marta', 'Marta not found :('));
    $menu1->addItem(5, 'Finish session', ItrItem::finishSession('Finish session'));
    $menu1->addItem(6, 'Exec function', ItrItem::execFunction(function($context){
        $result = restCommand('imbot.message.add', Array(
            "DIALOG_ID" => $_REQUEST['data']['PARAMS']['DIALOG_ID'],
            "MESSAGE" => 'Function executed (action)',
        ), $_REQUEST["auth"]);
        writeToLog($result, 'Exec function');
    }, 'Function executed (text)'));
    $menu1->addItem(9, 'Back to main menu', ItrItem::openMenu(0));

    $itr = new Itr($portalId, $dialogId, 0, $userId);
    $itr->addMenu($menu0);
    $itr->addMenu($menu1);
    $itr->run(prepareText($message));

    return true;
}
```