# Widgets in Messenger: Overview of Embedding Points

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This section describes the embedding points for widgets in the Bitrix24 chat interface. Through these points, developers can add a button to the panel above the input field, a separate item in the chat sidebar, an action in the context menu of a specific message, or a section of their own in the messenger navigation menu.

To register a widget, use the method [placement.bind](../placement-bind.md) and pass the required code in the `PLACEMENT` parameter.

> Quick navigation: [all embedding points](#all-placements)

## How to Choose an Embedding Point

The choice of embedding point depends on what the user will be interacting with in the chat interface.

- trigger an action while working with the current chat — [IM_TEXTAREA](./textarea.md)
- show a separate screen with additional information or tools related to the chat — [IM_SIDEBAR](./sidebar.md)
- tie an action to a specific chat message — [IM_CONTEXT_MENU](./context-menu.md)
- open a separate section of the messenger that is not tied to a chat — [IM_NAVIGATION](./navigation.md)

The mobile application chat is a separate embedding point, [IMMOBILE_CONTEXT_MENU](../mobile-app.md): it also receives `dialogId`, but it lives in the mobile messenger and is not displayed on the web.

## How to Get Started

1. Choose the embedding point for your scenario: input panel, sidebar, message menu, or navigation menu.
2. Register the handler with the [placement.bind](../placement-bind.md) method and pass the required code in the `PLACEMENT` parameter. The method is available to an administrator only and requires the application context: an embedding point cannot be bound with a webhook. On successful registration, the method returns `result: true` — the response breakdown and the error codes are on its page.
3. Pass the icon name in `OPTIONS` if the embedding point requires it, and the display restrictions if you need them.
4. Complete the application installation. Until then, the widget is not displayed in the interface.
5. Open a chat and call the widget.
6. Parse `PLACEMENT_OPTIONS` in the handler to obtain the call context: the identifier of the current chat or message, if the embedding point passes them.
7. If necessary, use the obtained identifiers to call the REST API or open an additional interface.

## What the Handler Receives

Data is sent in a POST request: some parameters come in the handler URL query string, the rest in the request body {.b24-info}

The example is shown for the panel above the input field. For the other embedding points, only the `PLACEMENT` value and the call context in `PLACEMENT_OPTIONS` change — their composition is given in the table below.

```php
Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => de
    [APP_SID] => 99c80eff6378726287350416ee5fef0
    [AUTH_ID] => 6061e72600631fcd00005a4b00000001f0f1076700000000f69dd5fc643d9ce2fdbc1
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 50e00aa340631fcd00005a4b00000001f0f1071111116580a5b83c2de639ef28c12
    [SERVER_ENDPOINT] => https://oauth.bitrix.info/rest/
    [APPLICATION_TOKEN] => ec1b2074a9d3f5c81b6e40d27a95cf38
    [APPLICATION_SCOPE] => im,placement
    [member_id] => da45a03b265ed12127f8a258d793cc5d
    [status] => L
    [PLACEMENT] => IM_TEXTAREA
    [PLACEMENT_OPTIONS] => {"dialogId":"chat2","URI":"\/online\/"}
)
```

After parsing, the `PLACEMENT_OPTIONS` string from this example looks like this:

```json
{
    "dialogId": "chat2",
    "URI": "/online/"
}
```

{% include [Note on required parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is passed as a JSON string with the context of the call.

#|
|| **Embedding Point** | **Keys** | **Description** ||
|| [IM_TEXTAREA](./textarea.md) | `dialogId`, `URI` | Identifier of the current chat ||
|| [IM_SIDEBAR](./sidebar.md) | `dialogId`, `URI` | Identifier of the current chat ||
|| [IM_CONTEXT_MENU](./context-menu.md) | `dialogId`, `messageId`, `URI` | Identifier of the current chat and selected message ||
|| [IM_NAVIGATION](./navigation.md) | `URI` | The widget is opened for the messenger as a whole, not for a particular chat ||
|#

The `URI` key is passed to every embedding point of the web messenger and contains the address of the page the widget is opened from — for the messenger this is `/online/`. The mobile point `IMMOBILE_CONTEXT_MENU` does not receive it: there the widget opens as a separate page rather than as a frame inside the interface.

## OPTIONS When Registering via placement.bind

The messenger embedding points accept the `OPTIONS` parameter of the [placement.bind](../placement-bind.md) method. The set of supported keys differs from point to point.

#|
|| **Embedding Point** | **Supported `OPTIONS` Keys** ||
|| [IM_TEXTAREA](./textarea.md) | `iconName`*, `extranet`, `context`, `role`, `color`, `width`, `height` ||
|| [IM_SIDEBAR](./sidebar.md) | `iconName`*, `extranet`, `context`, `role`, `color` ||
|| [IM_CONTEXT_MENU](./context-menu.md) | `extranet`, `context`, `role` ||
|| [IM_NAVIGATION](./navigation.md) | `iconName`*, `extranet`, `role` ||
|#

The key marked with an asterisk is required: without `iconName`, the `placement.bind` method returns the `ERROR_ARGUMENT` error.

The `context` key restricts the display to a chat type. Possible values: `ALL` — all chats, `USER` — personal user chats except chats with bots, `CHAT` — group chats except `LINES` and `CRM`, `LINES` — open channel chats, `CRM` — chats created within CRM. Several values are passed separated by `;`. The `IM_NAVIGATION` point does not support this key: a navigation item is not tied to a chat.

The `role` and `extranet` keys determine which categories of users have access to the widget. The full description of all keys with their types and default values is on the page of each embedding point.

The table lists the active embedding points. The archive `IM_SMILES_SELECTOR` accepts `context`, `role`, and `extranet`, but its handler is not called — [more details on its page](./smile-selector.md).

## Relationships with Other Objects

**Chat.** The `dialogId` parameter in `PLACEMENT_OPTIONS` indicates which chat the handler was invoked for. You can retrieve information about the chat by its identifier using the method [im.dialog.get](../../chats/im-dialog-get.md).

**User.** In a personal conversation, `dialogId` equals the identifier of the interlocutor, so their data is returned by the [user.get](../../user/user-get.md) method.

**Message.** The `messageId` parameter indicates which chat message the handler was invoked for. It arrives only in the `IM_CONTEXT_MENU` embedding point. Using this identifier, the application works with the message [via the methods of the section](../../chats/messages/index.md) — for example, updates it with the [im.message.update](../../chats/messages/im-message-update.md) method or deletes it with the [im.message.delete](../../chats/messages/im-message-delete.md) method.

## Typical Errors

#|
|| **Error** | **How to Resolve** ||
|| `placement.bind` returns `WRONG_AUTH_TYPE` with the description `Application context required` | Register the embedding point on behalf of an application. An embedding point cannot be bound with a webhook ||
|| `placement.bind` returns `ERROR_ARGUMENT` | A required parameter is missing. For `IM_TEXTAREA`, `IM_SIDEBAR`, and `IM_NAVIGATION`, `iconName` is required in `OPTIONS`. The code of the empty field arrives in `argument` ||
|| Widget does not display after registration | Complete the application installation and reopen the chat ||
|| Widget does not appear in the interface because the script is checked outside the chat | Check the embedding only in the chat interface ||
|| Registration via `placement.bind` fails due to an invalid value for `OPTIONS.context` | Use only valid values `ALL`, `USER`, `CHAT`, `LINES`, `CRM` ||
|| The restriction by `context` does not work as expected because `ALL` was passed along with other values | Pass only `ALL` or a list of specific contexts separated by `;` ||
|#

The error arrives in the response body — the code in the `error` field, the text in `error_description`:

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Current authorization type is denied for this method Application context required"
}
```

Other registration error codes are listed in the "Possible Error Codes" section of the [placement.bind](../placement-bind.md) page.

## Overview of Embedding Points {#all-placements}

> Scope: [`placement, im`](../../scopes/permissions.md)

The `placement` scope is required to register the handler. To work with a chat from the widget — for example, to get it by the `dialogId` from the call context — the application also needs the `im` scope. The exception is the archive `IM_SMILES_SELECTOR`: it is declared in the global `placement` scope and requires no second scope.

#|
|| **Embedding Point** | **When to Use** ||
|| [IM_TEXTAREA](./textarea.md) | Item in the panel above the input field ||
|| [IM_SIDEBAR](./sidebar.md) | Item in the chat sidebar ||
|| [IM_CONTEXT_MENU](./context-menu.md) | Item in the context menu of a message ||
|| [IM_NAVIGATION](./navigation.md) | Item in the messenger navigation menu ||
|| [IM_SMILES_SELECTOR](./smile-selector.md) | Archive page of the deprecated placement. Do not use for new integrations ||
|#

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../placements.md)
- [{#T}](../placement-bind.md)
- [{#T}](../placement-get.md)
- [{#T}](../placement-list.md)
- [{#T}](../placement-unbind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../bx24-widget-methods.md)
- [{#T}](../../chats/index.md)
- [{#T}](../../../settings/interactivity/index.md)
