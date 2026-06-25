# Open Channels: Common Scenarios

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A scenario is a sequence of requests for a single task in Open Channels. Scenarios help connect external channels, pass customer Salutations to Bitrix24, and return operator responses to the same channel.

An Open Channel defines the rules for processing a Salutation within Bitrix24: operator queues, routing, CRM mode, and other dialog configurations. A connector links an Open Channel to an external channel, such as a website chat or another service.

> Quick links: [Scenario and Methods](#choose-tutorial)
>
> User Documentation: [Create and configure Open Channels](https://helpdesk.bitrix24.com/open/25385203/)

## Connection with Other Objects

A scenario with an external chat is built around a connector, an external chat, and an application handler.

- **Connector.** The connector links an external channel to an Open Channel. An application registers it using the [imconnector.register](../../api-reference/imopenlines/imconnector/imconnector-register.md) method, after which Bitrix24 displays the connector in the Contact Center settings. When registering, set the connector code in the `ID` parameter, then pass this value in the parameter `CONNECTOR`.
- **External Chat.** An external website or service stores the public part of the conversation: the chat interface, the visitor identifier, and the message history. A customer message is passed to Bitrix24 using the [imconnector.send.messages](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md) method. In the `MESSAGES` array, the application passes customer data, messages, and chat information.
- **Application Handler.** The handler receives events from Bitrix24. When an operator responds from an Open Channel, the application receives the [OnImConnectorMessageAdd](../../api-reference/imopenlines/imconnector/events/on-im-connector-message-add.md) event, sends the response to the external chat, and confirms delivery using the [imconnector.send.status.delivery](../../api-reference/imopenlines/imconnector/imconnector-send-status-delivery.md) method. In [imconnector.register](../../api-reference/imopenlines/imconnector/imconnector-register.md), pass `PLACEMENT_HANDLER` for the connector configuration interface, and in [event.bind](../../api-reference/events/event-bind.md), pass `handler` for the event.

## Getting Started

1. Define the external channel: a website chat, a contact form, or another service where the customer starts a conversation.
2. Create a Bitrix24 application and prepare public HTTPS URLs: one for the connector configuration interface and one for the event handler.
3. Register the connector using the [imconnector.register](../../api-reference/imopenlines/imconnector/imconnector-register.md) method.
4. Subscribe the application to the [OnImConnectorMessageAdd](../../api-reference/imopenlines/imconnector/events/on-im-connector-message-add.md) event using the [event.bind](../../api-reference/events/event-bind.md) method.
5. Connect the connector to the required Open Channel in Bitrix24. In the application configuration handler, call [imconnector.activate](../../api-reference/imopenlines/imconnector/imconnector-activate.md) and pass `ACTIVE=1`.
6. Pass customer messages using the [imconnector.send.messages](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md) method.
7. Receive operator responses in the event handler and confirm delivery using the [imconnector.send.status.delivery](../../api-reference/imopenlines/imconnector/imconnector-send-status-delivery.md) method.

## Key Considerations

- The connector operates through an application. Webhooks are not suitable: the application must register the connector, receive events, and work with Open Channel configurations.
- To use connector methods and subscribe to the [OnImConnectorMessageAdd](../../api-reference/imopenlines/imconnector/events/on-im-connector-message-add.md) event, the application requires the [`imopenlines`](../../api-reference/scopes/permissions.md) scope.
- Use the same connector code during registration, activation, message sending, and event handling.

## How to Select a Scenario {#choose-tutorial}

#|
|| **If necessary** | **Open** ||
|| Create an online chat on the website and pass messages to an operator in Bitrix24 | [How to create an Open Channels connector for a website chat](./example-connector.md) ||
|| Clarify method parameters, events, and the connector data structure | [Open Channels connectors](../../api-reference/imopenlines/imconnector/index.md) ||
|| Configure lines, sessions, operators, messages, and chatbots | [Open Channels](../../api-reference/imopenlines/openlines/index.md) ||
|#
