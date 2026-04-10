# Chat Widgets: Overview of Embedding Points

This section describes the embedding points for widgets in the Bitrix24 chat interface. Through these points, developers can add a button to the panel above the input field, a separate item in the chat sidebar, or an action in the context menu of a specific message.

To register a widget, use the method [placement.bind](../placement-bind.md) and pass the required code in the `PLACEMENT` parameter.

> Quick navigation: [all widgets](#all-widgets)

## How to Choose an Embedding Point

The choice of embedding point depends on what the user will be interacting with in the chat interface.

- The user should trigger an action while working with the current chat — [IM_TEXTAREA](./textarea.md).
- A separate screen with additional information or tools related to the chat is needed — [IM_SIDEBAR](./sidebar.md).
- The action needs to be tied to a specific chat message — [IM_CONTEXT_MENU](./context-menu.md).

## How to Get Started

1. Choose the embedding point for your scenario: input panel, sidebar, or message menu.
2. Register the handler via [placement.bind](../placement-bind.md) and pass the appropriate `PLACEMENT`.
3. Parse `PLACEMENT_OPTIONS` in the handler to obtain the context of the current chat or message.
4. If necessary, use the obtained identifiers to call the REST API or open an additional interface.

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

{% list tabs %}

- IM_TEXTAREA

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
        [member_id] => da45a03b265ed12127f8a258d793cc5d
        [status] => F
        [PLACEMENT] => IM_TEXTAREA
        [PLACEMENT_OPTIONS] => {"dialogId":"chat1489"}
    )
    ```

- IM_SIDEBAR

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
        [member_id] => da45a03b265ed12127f8a258d793cc5d
        [status] => F
        [PLACEMENT] => IM_SIDEBAR
        [PLACEMENT_OPTIONS] => {"dialogId":"chat1489"}
    )
    ```

- IM_CONTEXT_MENU

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
        [member_id] => da45a03b265ed12127f8a258d793cc5d
        [status] => F
        [PLACEMENT] => IM_CONTEXT_MENU
        [PLACEMENT_OPTIONS] => {"messageId":84889, "dialogId":"chat1489"}
    )
    ```

{% endlist %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is passed as a JSON string with the context of the call.

#|
|| **Widget** | **Keys** | **Description** ||
|| [IM_TEXTAREA](./textarea.md) | `dialogId` | Identifier of the current chat ||
|| [IM_SIDEBAR](./sidebar.md) | `dialogId` | Identifier of the current chat ||
|| [IM_CONTEXT_MENU](./context-menu.md) | `dialogId`, `messageId` | Identifier of the current chat and selected message ||
|#

## Connection with Other Objects

**Chat.** The `dialogId` parameter in `PLACEMENT_OPTIONS` indicates which chat the handler was invoked for. You can retrieve information about the chat by its identifier using the method [im.dialog.get](../../chats/im-dialog-get.md).

**Message.** The `messageId` parameter indicates which chat message the handler was invoked for. It is used only in the `IM_CONTEXT_MENU` widget.

**Chat Type.** The `OPTIONS.context` parameter links the embedding to the chat type:

- `USER` — personal user chats
- `CHAT` — group chats
- `LINES` — open channel chats
- `CRM` — CRM chats
- `ALL` — all chats

**Access Permissions.** The `role` and `extranet` parameters determine which categories of users have access to the widget.

## Typical Errors

#|
|| **Error** | **How to Resolve** ||
|| Widget does not display after registration | Complete the application installation and reopen the chat ||
|| Widget does not appear in the interface because the script is checked outside the chat | Check the embedding only in the chat interface ||
|| Registration via `placement.bind` fails due to an invalid value for `OPTIONS.context` | Use only valid values `ALL`, `USER`, `CHAT`, `LINES`, `CRM` ||
|| The restriction by `context` does not work as expected because `ALL` was passed along with other values | Pass only `ALL` or a list of specific contexts separated by `;` ||
|#

## Overview of Widgets {#all-widgets}

> Scope: [`im`](../../scopes/permissions.md)

#|
|| **Widget** | **When to Use** ||
|| [IM_TEXTAREA](./textarea.md) | Item in the panel above the input field ||
|| [IM_SIDEBAR](./sidebar.md) | Item in the chat sidebar ||
|| [IM_CONTEXT_MENU](./context-menu.md) | Item in the context menu of a message ||
|| [IM_SMILES_SELECTOR](./smile-selector.md) | Archive page of the deprecated embedding. Do not use for new integrations ||
|#