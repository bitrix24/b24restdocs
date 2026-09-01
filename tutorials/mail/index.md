# Mail: Common Scenarios

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Tutorials help you choose methods for common mail tasks: send an e-mail from a connected mailbox, find an incoming e-mail, and create a CRM activity from it.

A tutorial describes one practical task and the order of methods required to complete it.

> Quick navigation: [all tutorials](#choose-tutorial)

## Links to Other Objects

- **Mailboxes and senders.** To send an e-mail, first retrieve the available senders using [mail.mailbox.senders](../../api-reference/mail/mailbox/mail-mailbox-senders.md). The method returns addresses from mailboxes available to the webhook or application user
- **Recipients.** Before sending, you can find a contact by name, phone number, or e-mail using [mail.recipient.listcontacts](../../api-reference/mail/recipient/mail-recipient-listcontacts.md). In the tutorial, the search result is used to select the recipient address
- **E-mails and CRM activities.** Incoming e-mails can be retrieved using [mail.message.list](../../api-reference/mail/message/mail-message-list.md). The [mail.message.createcrmactivity](../../api-reference/mail/message/mail-message-createcrmactivity.md) method creates a CRM activity from an e-mail if the user has access to the mailbox and CRM

{% note info "" %}

Mail methods belong to REST 3.0. They must be called using an address with the `/rest/api/` segment, and the request body must be passed in JSON format. Method call specifics and the JSON request format are described in the [REST 3.0 overview](../../api-reference/rest-v3.md).

{% endnote %}

## How to Get Started

1. Create an [incoming webhook](../../local-integrations/local-webhooks.md#incoming-webhook) or an application with the `mail` scope
2. Check that the webhook user can see the required mailbox
3. If you need to create an activity from an e-mail, check the user's CRM access
4. Choose a tutorial in the [How to Choose a Tutorial](#choose-tutorial) table
5. Execute the methods in the order described in the tutorial

## How to Choose a Tutorial {#choose-tutorial}

#|
|| **If You Need To** | **Open** ||
|| Send an e-mail to a client from a connected mailbox | [How to Send an E-mail from a Connected Mailbox](./how-to-send-email-from-mailbox.md) ||
|| Find an incoming e-mail and create a CRM activity from it | [How to Create a CRM Activity from an Incoming E-mail](./how-to-create-crm-activity-from-email.md) ||
|| View the mail method reference | [Mail in REST 3.0: Section Overview](../../api-reference/mail/index.md) ||
|#
