# Email Services: Overview of Methods

In Bitrix24, you can connect mailboxes from services like Gmail and others. This simplifies working with emails.

> Quick navigation: [all methods](#all-methods)

You can manage only email services through the REST API. Connecting mailboxes, sending, and receiving emails must be done through the Bitrix24 interface.

{% note tip "User Documentation" %}

- [How to connect Gmail to Bitrix24](https://helpdesk.bitrix24.com/open/18508706/)
- [Ways to connect mailboxes in Bitrix24](https://helpdesk.bitrix24.com/open/19264454/)
- [How to work with email in Bitrix24](https://helpdesk.bitrix24.com/open/20134658/)
- [Questions about email operation (connection, integration with CRM)](https://helpdesk.bitrix24.com/open/8293717/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`mailservice`](../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [mailservice.fields](./mailservice-add.md) | Returns the description of the email service fields ||
|| [mailservice.list](./mailservice-list.md) | Returns a list of all email services ||
|| [mailservice.get](./mailservice-get.md) | Returns the parameters of the specified email service ||
|| [mailservice.add](./mailservice-add.md) | Adds an email service ||
|| [mailservice.update](./mailservice-update.md) | Updates the parameters of the email service ||
|| [mailservice.delete](./mailservice-delete.md) | Deletes an email service ||
|#