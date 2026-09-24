# Open Channel Chats and CRM: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Open channel chats store conversations with customers from online chat, messengers, and social networks. Bitrix24 can link a dialog with a customer to a lead, deal, contact, or company. The `imopenlines.crm.chat.*` methods find chats by CRM object, add employees and chatbots to them, or remove participants. For example, an application can bring the manager responsible for a deal into the conversation with the customer.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Use chats in Open Channels](https://helpdesk.bitrix24.com/open/25967455/)

## How a Chat Is Linked to a CRM Object

When an open channel dialog is added to CRM, Bitrix24 creates an activity in a [lead](../../../crm/leads/index.md), [deal](../../../crm/deals/index.md), [contact](../../../crm/contacts/index.md), or [company](../../../crm/companies/index.md). The methods use this activity to find the chat. If the object has no such activity, the methods do not find the chat.

In a request, the CRM object, the chat, and the participant are specified as follows:

#|
|| **Parameter** | **Meaning** | **Example value** ||
|| `CRM_ENTITY_TYPE` | CRM object type: `lead`, `deal`, `contact`, or `company`. The methods do not support invoices and smart processes | `contact` ||
|| `CRM_ENTITY` | CRM object ID. You can retrieve it using the [crm.item.list](../../../crm/universal/crm-item-list.md) method | `2389` ||
|| `CHAT_ID` | Chat ID. It is returned by the [imopenlines.crm.chat.get](./imopenlines-crm-chat-get.md) and [imopenlines.crm.chat.getLastId](./imopenlines-crm-chat-get-last-id.md) methods | `1971` ||
|| `USER_ID` | Participant ID — an employee or a chatbot. You can find an employee using the [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md) methods | `15` ||
|#

By default, the [imopenlines.crm.chat.get](./imopenlines-crm-chat-get.md) method returns only chats in which an operator has accepted the dialog and has not finished it yet. To retrieve all chats linked to the object, pass `ACTIVE_ONLY` with the value `N`. The [imopenlines.crm.chat.getLastId](./imopenlines-crm-chat-get-last-id.md) method returns the last chat of the object, even if its dialog is finished.

Participants can be changed only in the chats that [imopenlines.crm.chat.get](./imopenlines-crm-chat-get.md) returns by default. For a finished chat, the [imopenlines.crm.chat.user.add](./imopenlines-crm-chat-user-add.md) and [imopenlines.crm.chat.user.delete](./imopenlines-crm-chat-user-delete.md) methods return the `CHAT_NOT_IN_CRM` error.

## Linking Chats with Other Objects

Besides CRM, chats are linked to chatbots, conversation history, open channels, and connectors.

**Chatbots.** A bot in a chat can reply to the customer, transfer the dialog to an operator or to the queue, and finish it. The [imopenlines.bot.*](../chat-bots/index.md) method group handles this.

**Conversation History.** It is returned by the [imopenlines.session.history.get](../sessions/imopenlines-session-history-get.md) method — pass `CHAT_ID` to it.

**Open Channels.** You can create, configure, or delete the channel itself using the [imopenlines.*](../index.md) methods.

**Connectors.** A customer writes to an open channel through a connector, for example, Telegram or an online chat on a website. You can connect and configure a connector using the [imconnector.*](../../imconnector/index.md) methods.

## How to Work with Chats

1. Find the chat linked to the CRM object: pass `CRM_ENTITY_TYPE` and `CRM_ENTITY` to the [imopenlines.crm.chat.get](./imopenlines-crm-chat-get.md) method without `ACTIVE_ONLY`. The method returns the chats in which participants can be changed. Save `CHAT_ID`
2. Add an employee or a chatbot to the chat using the [imopenlines.crm.chat.user.add](./imopenlines-crm-chat-user-add.md) method: pass the same `CRM_ENTITY_TYPE` and `CRM_ENTITY`, the participant ID in `USER_ID`, and the chat ID in `CHAT_ID`. To remove a participant, use the [imopenlines.crm.chat.user.delete](./imopenlines-crm-chat-user-delete.md) method with the same parameters
3. Write to the customer on behalf of an employee or a bot using the [imopenlines.crm.message.add](../messages/imopenlines-crm-message-add.md) method

If the dialog is not linked to CRM yet, create a lead from it using the [imopenlines.crm.lead.create](../sessions/imopenlines-crm-lead-create.md) method — it also needs `CHAT_ID`. If the dialog is already linked, the method creates nothing but returns `true`.

{% note tip "User Documentation" %}

- [How to create and configure an open channel](https://helpdesk.bitrix24.com/open/25385203/)

- [Contact Center](https://helpdesk.bitrix24.com/open/24095446/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method — [imopenlines.crm.chat.get](./imopenlines-crm-chat-get.md), [imopenlines.crm.chat.user.add](./imopenlines-crm-chat-user-add.md), and [imopenlines.crm.chat.user.delete](./imopenlines-crm-chat-user-delete.md) require read access to the CRM object

#|
|| **Method** | **Description** ||
|| [imopenlines.crm.chat.get](./imopenlines-crm-chat-get.md) | Retrieves chats for the CRM object ||
|| [imopenlines.crm.chat.getLastId](./imopenlines-crm-chat-get-last-id.md) | Retrieves the ID of the last chat for the CRM object ||
|| [imopenlines.crm.chat.user.add](./imopenlines-crm-chat-user-add.md) | Adds a user to an existing chat of the CRM object ||
|| [imopenlines.crm.chat.user.delete](./imopenlines-crm-chat-user-delete.md) | Removes a user from the chat of the CRM object ||
|#
