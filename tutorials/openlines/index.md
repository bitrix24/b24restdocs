# Typical Use-Cases and Scenarios in Open Channels

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This section contains practical scenarios for working with open channels via the REST API. The materials describe the complete integration process: from registering the connector to exchanging messages between the external channel and Bitrix24. An implementation example is available in the article [How to Create an Open Channels Connector for Website Chat](./example-connector.md).

A reference guide for methods, events, and parameters is available in the section [Open Channels Connectors](../../api-reference/imopenlines/imconnector/index.md).

## Open Channels Connector

The connector links an external channel with an open line. The external service transmits client messages to Bitrix24. Employee responses are sent back to this service.

This approach is suitable for cases where the integration manages the public part of the chat and maintains the dialogue state on its side. The website sends messages using the method [imconnector.send.messages](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md). The event handler receives responses from Bitrix24 and confirms delivery via [imconnector.send.status.delivery](../../api-reference/imopenlines/imconnector/imconnector-send-status-delivery.md).

## Connection Steps

The article about the connector describes the complete cycle of connecting an external chat.

1. Register the connector in Bitrix24 using the method [imconnector.register](../../api-reference/imopenlines/imconnector/imconnector-register.md).
2. Subscribe the application to the event [OnImConnectorMessageAdd](../../api-reference/imopenlines/imconnector/events/on-im-connector-message-add.md) for new messages.
3. Activate the connector for the selected open line.
4. Transmit visitor messages from the website to Bitrix24.
5. Receive operator responses and confirm delivery.

## Technical Requirements

- Use application authorization. The example utilizes the [CRest PHP SDK](../../sdk/crest-php-sdk/index.md). Webhooks are not suitable because the connector is registered as part of the application, receives events, and works with open line settings.
- Host the event handler on a server accessible via HTTPS.
- Store the dialogue history in a database for reliability.
- Record the connector ID and use it when registering, sending messages, and processing events.