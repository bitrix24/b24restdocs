# Local Integrations: Use Case Scenarios

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A scenario describes a practical task and the sequence of methods required to perform it. In local integrations, scenarios demonstrate how to pass data to the CRM, create a task with an attachment, embed an application interface, and configure a chatbot.

> Quick links: [All Scenarios](#choose-tutorial)
>
> User documentation: [Create webhooks and apps in Bitrix24](https://helpdesk.bitrix24.com/open/21133100/)

## Connection with Other Objects

Scenarios are linked to CRM objects, tasks, Drive files, the Bitrix24 interface, and chats.

- **CRM.** Data from an external form can be retained in a contact using the [crm.contact.add](../api-reference/crm/contacts/crm-contact-add.md) method. To migrate existing data, use the [crm.item.import](../api-reference/crm/universal/import/crm-item-import.md) and [crm.item.batchImport](../api-reference/crm/universal/import/crm-item-batch-import.md) methods.
- **Tasks and Drive Files.** A file is first uploaded using the [disk.folder.uploadfile](../api-reference/disk/folder/disk-folder-upload-file.md) method, and then its identifier is passed when creating a task using the [tasks.task.add](../api-reference/tasks/tasks-task-add.md) method.
- **Bitrix24 Interface.** A widget opens an application page within a CRM card. To register a handler, use the [placement.bind](../api-reference/widgets/placement-bind.md) method. This method works only within the context of an application.
- **Chats and Chatbots.** A chatbot is registered using the [imbot.v2.Bot.register](../api-reference/chat-bots/chat-bots-v2/imbot.v2/bots/bot-register.md) method. To receive events, you can configure periodic polling or specify a handler URL. The [imbot.v2](../api-reference/chat-bots/chat-bots-v2/index.md#all-methods) methods are available via an incoming webhook or a local application.

For more details on these tools, read the [Incoming and Outgoing Webhooks](./local-webhooks.md) and [Local Applications](./local-apps.md) articles.

## Getting Started

1. Define the integration task.
2. Select a scenario and tool in the [table](#choose-tutorial).
3. Verify the `scope` of the methods and the permissions of the user on whose behalf the integration performs requests.
4. Prepare the data and identifiers. If Bitrix24 needs to open an application page or send an event to it, prepare a handler—a page with a URL accessible from an external network.
5. Execute the methods in the specified order and verify the result.

## How to Choose a Scenario {#choose-tutorial}

#|
|| **If needed** | **Tool** | **Open** ||
|| Transfer contacts from a website web form to CRM | Incoming webhook or local application | [Add contact via web form](../tutorials/crm/how-to-add-crm-objects/how-to-add-contact.md) ||
|| Add application interface to a CRM card | Local application | [How to embed widgets into CRM](../tutorials/crm/crm-widgets/index.md) ||
|| Migrate existing data to CRM | Incoming webhook or local application | [Data import to CRM: methods overview](../api-reference/crm/universal/import/index.md) ||
|| Create a task and attach a file to it | Incoming webhook or local application | [How to create a task with an attached file](../tutorials/tasks/how-to-create-task-with-file.md) ||
|| Create a chatbot for Bitrix24 | Incoming webhook or local application | [Chatbots 2.0: quick start](../api-reference/chat-bots/chat-bots-v2/quick-start.md) ||
|#
