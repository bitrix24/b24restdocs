# Open Channels: Common Scenarios

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A scenario is a sequence of requests for a single task in Open Channels. Scenarios help connect external channels, transfer customer inquiries to Bitrix24, return operator responses to the same channel, and find CRM objects linked to conversations.

An Open Channel defines the rules for processing an inquiry within Bitrix24: operator queues, routing, operating modes, and other conversation configurations. A connector links an Open Channel to an external channel, such as a website chat or another service.

> Quick links: [Scenario and Methods](#choose-tutorial)
>
> User Documentation: [Create and configure Open Channels](https://helpdesk.bitrix24.com/open/25385203/)

## Connection with Other Objects

A scenario with an external chat is built around a connector, an external chat, and an application handler.

- **Connector.** An application registers the connector using the [imconnector.register](../../api-reference/imopenlines/imconnector/imconnector-register.md) method, after which it appears in the Contact Center settings. When registering, set the connector code in lowercase in the `ID` parameter, then pass this code in the `CONNECTOR` parameter.
- **External Chat.** An external website or service stores the public part of the conversation: the chat interface, the visitor identifier, and the message history. A customer message is transferred to Bitrix24 using the [imconnector.send.messages](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md) method. In the `MESSAGES` array, the application passes customer data, messages, and chat information.
- **Application Handler.** The handler receives events from Bitrix24. When an operator responds from an Open Channel, the application receives the [OnImConnectorMessageAdd](../../api-reference/imopenlines/imconnector/events/on-im-connector-message-add.md) event, sends the response to the external chat, and confirms delivery using the [imconnector.send.status.delivery](../../api-reference/imopenlines/imconnector/imconnector-send-status-delivery.md) method. The event contains the connector code `CONNECTOR`, the line `LINE`, and the `MESSAGES` array: the response text, the external `chat.id`, and the internal `im.chat_id` and `im.message_id` — pass them to the delivery confirmation. In [imconnector.register](../../api-reference/imopenlines/imconnector/imconnector-register.md), pass `PLACEMENT_HANDLER` for the connector configuration interface, and in [event.bind](../../api-reference/events/event-bind.md), pass `handler` for the event.
- **CRM.** The CRM tracker can create a lead or a deal with a contact from an inquiry, or find an existing contact or company, and link them to the conversation. The value of the Messenger field of such an object is built from the Open Channel user code, and you can find the object by it using the [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) method.

## Getting Started

To connect an external channel, follow the steps below. If the connector is already running and you need to find a linked CRM object, go to the [search scenario](./find-crm-object-by-dialog.md).

1. Define the external channel: a website chat, a contact form, or another service where the customer starts a conversation.
2. Create a Bitrix24 application and prepare public URLs with HTTPS access: one for the connector configuration interface and one for the event handler.
3. Register the connector using the [imconnector.register](../../api-reference/imopenlines/imconnector/imconnector-register.md) method.
4. Subscribe the application to the [OnImConnectorMessageAdd](../../api-reference/imopenlines/imconnector/events/on-im-connector-message-add.md) event using the [event.bind](../../api-reference/events/event-bind.md) method.
5. Connect the connector to the required Open Channel in Bitrix24. In the application configuration handler, call [imconnector.activate](../../api-reference/imopenlines/imconnector/imconnector-activate.md) and pass `ACTIVE=1`.
6. Transfer customer messages using the [imconnector.send.messages](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md) method.
7. Receive operator responses in the event handler and confirm delivery using the [imconnector.send.status.delivery](../../api-reference/imopenlines/imconnector/imconnector-send-status-delivery.md) method.

## Key Considerations

- The connector operates through an application. Webhooks are not suitable: the application must register the connector, receive events, and work with Open Channel configurations.
- For connector methods and subscribing to the [OnImConnectorMessageAdd](../../api-reference/imopenlines/imconnector/events/on-im-connector-message-add.md) event, the application requires the [`imopenlines`](../../api-reference/scopes/permissions.md) scope.
- Use the same connector code during registration, activation, message sending, and event handling.

## How to Select a Scenario {#choose-tutorial}

#|
|| **If needed** | **Open** | **Result** ||
|| Create an online chat on the website and pass messages to an operator in Bitrix24 | [How to create an Open Channels connector for a website chat](./example-connector.md) | A visitor message opens a conversation in the Open Channel, the operator response arrives in the website chat, and delivery is confirmed with the `imconnector.send.status.delivery` method ||
|| Find a lead, contact, or company that the CRM tracker linked to an Open Channel dialogue | [How to find a CRM object created from an Open Channel dialogue](./find-crm-object-by-dialog.md) | The lead, contact, or company ID in `items[0].id` of the `crm.item.list` response ||
|| Clarify method parameters, events, and the connector data structure | [Open Channels connectors](../../api-reference/imopenlines/imconnector/index.md) | Descriptions of parameters, responses, and errors of the `imconnector.*` methods and connector events ||
|| Configure lines, sessions, operators, messages, and chatbots | [Open Channels](../../api-reference/imopenlines/openlines/index.md) | Descriptions of the `imopenlines.*` methods ||
|#
