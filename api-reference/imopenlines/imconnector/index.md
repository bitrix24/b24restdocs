# Open Channels Connectors: Overview of Methods and Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Open Channels connectors link an external communication channel to Bitrix24. Through a connector, the application registers an external channel, sends customer messages to an open channel, and receives events about messages, dialogues, connector disabling, and line deletion.

{% note info "" %}

The methods in this section work only in the context of an [application](../../../settings/app-installation/index.md). A call via a webhook returns an authorization error.

{% endnote %}

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Communication channels in Contact Center](https://helpdesk.bitrix24.com/open/25935795/)

## Connection of Connectors with Other Objects

A connector does not work on its own: it is attached to an open channel, and the correspondence in that channel is linked to a chat, employees, chatbots, and CRM objects.

**Open Channels.** A line receives messages from the connector, applies queue settings, and distributes dialogues between employees. Settings and sessions are managed by the [imopenlines.*](../openlines/index.md) method group, and the full scope of the module is collected in the [Open Channels](../index.md) section.

**CRM.** Open Channels dialogues are linked to leads, deals, contacts, and companies through the CRM tracker: it recognizes the customer contact details in incoming messages. The tracker can be disabled for an individual message with the `message.disable_crm` parameter of the [imconnector.send.messages](./imconnector-send-messages.md) method, and it does not run for connectors that group chats by `chat.id`. To create a lead from a dialogue manually, use the [imopenlines.crm.lead.create](../openlines/sessions/imopenlines-crm-lead-create.md) method.

**User.** This section deals with two different user identifiers. The interlocutor from the external channel is set by `user.id`, and the same value is passed in the `USER_ID` parameter of the [imconnector.chat.name.set](./imconnector-chat-name-set.md) method. A Bitrix24 employee is set by `message.user_id` of the [imconnector.send.messages](./imconnector-send-messages.md) method — the message is sent on their behalf. The employee identifier can be obtained using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods.

**Chat.** Correspondence between a customer and an employee is stored in the Open Channel chat. The external identifier `chat.id` links the external system chat to the Bitrix24 chat in methods for sending, updating, and deleting messages.

**Chatbots.** Bots can reply in a dialogue, transfer a conversation to an employee, and close a session. Use the [imopenlines.bot.*](../openlines/chat-bots/index.md) method group for bot actions in an open channel.

## How to Connect a Connector

1. Create an open channel using the [imopenlines.config.add](../openlines/imopenlines-config-add.md) method, or get the identifier of an existing line using the [imopenlines.config.list.get](../openlines/imopenlines-config-list-get.md) method
2. Register the connector using the [imconnector.register](./imconnector-register.md) method
3. Activate the connector on the line using the [imconnector.activate](./imconnector-activate.md) method
4. Set the connector configurations using the [imconnector.connector.data.set](./imconnector-connector-data-set.md) method
5. Check channel readiness using the [imconnector.status](./imconnector-status.md) method
6. Subscribe to the [connector events](./events/index.md) to receive open channel messages and pass them to the external channel

## Identifiers and Codes

**Connector code.** It is set in the `ID` parameter of the [imconnector.register](./imconnector-register.md) method and is retained in lowercase: `MyConnector` becomes `myconnector`. The same value is passed in the `CONNECTOR` parameter of the other methods and is returned in the keys of the [imconnector.list](./imconnector-list.md) response and in event data. A dot in the connector code is not allowed.

**Line identifier.** `LINE` is the numeric identifier of the open channel the connector is attached to, for example `107`. It can be obtained using the [imopenlines.config.list.get](../openlines/imopenlines-config-list-get.md) method. The connector state is retained separately for each connector-line pair: the connector can be enabled on one line and disabled on another.

**Chat and message identifiers.** The application sets the external identifiers `chat.id` and `message.id` itself and passes them in the message sending methods, for example `channel-123` and `ext-msg-1007`. The internal identifiers `im.chat_id` and `im.message_id` are created by Bitrix24, and the application receives them in the [OnImConnectorMessageAdd](./events/on-im-connector-message-add.md) event. The external identifiers do not replace the internal ones: both pairs of values are passed in the delivery status.

**Chat mode.** The `CHAT_GROUP` parameter of the [imconnector.register](./imconnector-register.md) method defines how the connector groups correspondence: by `chat.id` or by `user.id`. This mode also determines whether the `USER_ID` parameter is required in the [imconnector.chat.name.set](./imconnector-chat-name-set.md) method.

## Response Format

Every response contains the `time` block with the request execution time, and `result` comes in one of four forms:

- a boolean value — [imconnector.activate](./imconnector-activate.md) and [imconnector.connector.data.set](./imconnector-connector-data-set.md)
- an object with a nested `result` field — [imconnector.register](./imconnector-register.md) and [imconnector.unregister](./imconnector-unregister.md)
- an object with data — [imconnector.status](./imconnector-status.md) and [imconnector.list](./imconnector-list.md)
- an envelope with the `SUCCESS` and `DATA` fields — the message methods and [imconnector.chat.name.set](./imconnector-chat-name-set.md):

    ```json
    {
        "result": {
            "SUCCESS": true,
            "DATA": { "RESULT": [] }
        }
    }
    ```

    The composition of `DATA` depends on the method: for the message methods it is an object with the `RESULT` array, for the chat renaming method it is an object with an empty service `RESULT`, and for the delivery confirmation method it is an empty array.

Errors come in two forms:

- a system error — HTTP status `400` or `403`, with the `error` and `error_description` fields at the root of the response
- an application-level registration error — HTTP status `200`, with the `result: false`, `error`, and `error_description` fields inside `result`

The second form is returned by [imconnector.register](./imconnector-register.md) and [imconnector.unregister](./imconnector-unregister.md).

The methods for sending, updating, and deleting messages report a partial failure inside `DATA.RESULT`: each element has its own `SUCCESS` flag, and when `SUCCESS: false`, an `ERRORS` array with error texts. The delivery confirmation method provides no such report: it returns `SUCCESS: true` even if the status was not applied to the message.

## How to Work with Messages

Customer messages from the external channel are passed to the open channel by the [imconnector.send.messages](./imconnector-send-messages.md) method. Messages that have already been passed can be modified and deleted using the [imconnector.update.messages](./imconnector-update-messages.md) and [imconnector.delete.messages](./imconnector-delete-messages.md) methods.

The opposite direction works through events: the application receives [OnImConnectorMessageAdd](./events/on-im-connector-message-add.md), delivers the employee message to the external channel, and confirms the delivery using the [imconnector.send.status.delivery](./imconnector-send-status-delivery.md) method.

## Where the Application Appears in the Interface

**Connector settings page.** The main connector placement is [SETTING_CONNECTOR](../../widgets/setting-connector.md). Bitrix24 creates the binding itself during registration: the handler address is passed in the `PLACEMENT_HANDLER` parameter of the [imconnector.register](./imconnector-register.md) method, and there is no need to call [placement.bind](../../widgets/placement-bind.md) for it.

**Tile in the Contact Center.** To display a separate application tile in the Contact Center, use the placement code [CONTACT_CENTER](../../widgets/contact-center.md) and specify it in the `PLACEMENT` parameter of the [placement.bind](../../widgets/placement-bind.md) method.

## Overview of Methods and Events {#all-methods}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method
>
> Who can subscribe to events: any user

All methods in this section are available to any user, except [imconnector.list](./imconnector-list.md) — it requires the permission to modify Open Channels connectors.

All events in this section and the data the handler receives are described on the [Overview of Events](./events/index.md) page.

### Connector

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [imconnector.register](./imconnector-register.md) | Registers a connector ||
    || [imconnector.activate](./imconnector-activate.md) | Enables or disables a connector on a line ||
    || [imconnector.status](./imconnector-status.md) | Retrieves the connector status ||
    || [imconnector.connector.data.set](./imconnector-connector-data-set.md) | Sets connector configurations ||
    || [imconnector.list](./imconnector-list.md) | Retrieves a list of connectors ||
    || [imconnector.unregister](./imconnector-unregister.md) | Unregisters a connector ||
    |#

- Events

    #| 
    || **Event** | **Triggered** ||
    || [OnImConnectorLineDelete](./events/on-im-connector-line-delete.md) | When an open line is deleted ||
    || [OnImConnectorStatusDelete](./events/on-im-connector-status-delete.md) | When a connector is disabled on an open line ||
    |#

{% endlist %}

### Chats and Messages

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [imconnector.send.messages](./imconnector-send-messages.md) | Sends external channel messages to Bitrix24 ||
    || [imconnector.update.messages](./imconnector-update-messages.md) | Modifies sent messages ||
    || [imconnector.delete.messages](./imconnector-delete-messages.md) | Deletes sent messages ||
    || [imconnector.send.status.delivery](./imconnector-send-status-delivery.md) | Updates the `delivered` status ||
    || [imconnector.chat.name.set](./imconnector-chat-name-set.md) | Sets a new chat name ||
    |#

- Events

    #| 
    || **Event** | **Triggered** ||
    || [OnImConnectorDialogStart](./events/on-im-connector-dialog-start.md) | When a dialogue is created in an open channel ||
    || [OnImConnectorDialogFinish](./events/on-im-connector-dialog-finish.md) | When a dialogue is closed in an open channel ||
    || [OnImConnectorMessageAdd](./events/on-im-connector-message-add.md) | When a message is sent from an open channel to an external channel ||
    || [OnImConnectorMessageUpdate](./events/on-im-connector-message-update.md) | When a message is modified in an open channel ||
    || [OnImConnectorMessageDelete](./events/on-im-connector-message-delete.md) | When a message is deleted in an open channel ||
    |#

{% endlist %}

## Continue Learning

- [How to Create an Open Channels Connector for Website Chat](../../../tutorials/openlines/example-connector.md)
