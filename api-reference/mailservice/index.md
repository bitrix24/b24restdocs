# Mail Services: Methods Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A mail service is an entry in the Bitrix24 registry that holds mail server parameters: the name, the IMAP server address and port, the secure connection flag, the logo, and a link to the web interface. A user selects a service from this registry when connecting a mailbox. Bitrix24 already includes services such as Gmail, Outlook, and others, and the methods of this section allow you to add your own service to the registry.

The registry is retained separately for each site, so the methods only work with the services of the current site.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Connect mailboxes to Bitrix24](https://helpdesk.bitrix24.com/open/19264454/)

## How to Choose a Section

#|
|| **If You Need To** | **Open the Section** ||
|| Add a mail service or change its parameters | This section, the `mailservice.*` methods ||
|| Work with user mailboxes, emails, and recipients | [Webmail REST 3.0](../mail/index.md) ||
|#

The `mailservice.*` methods are called at `/rest/`: the section has no 3.0 version, and the `mail.*` methods of REST 3.0 do not manage mail services. Both REST versions work at the same time and do not replace each other — see the details in the [Overview of REST 3.0](../rest-v3.md).

{% note tip "How to Connect Mail in the Interface" %}

- [Connect Gmail mailbox to Bitrix24](https://helpdesk.bitrix24.com/open/25811739/)
- [Manage emails in Bitrix24](https://helpdesk.bitrix24.com/open/20134658/)

{% endnote %}

## Getting Started

1. Check which services already exist in Bitrix24 using the [mailservice.list](./mailservice-list.md) method
2. If the service you need is missing, create it using the [mailservice.add](./mailservice-add.md) method. The method returns the `ID` of the new service — retain this value
3. Update the service parameters using the [mailservice.update](./mailservice-update.md) method, or delete the service using the [mailservice.delete](./mailservice-delete.md) method
4. To retrieve the parameters of a single service by `ID`, use the [mailservice.get](./mailservice-get.md) method

`mailservice.list` returns active services only. `mailservice.get` also returns an inactive one, but you have to know its `ID`: there is no other way to retrieve the `ID` of an inactive service via REST.

## Relationships with Other Objects

A mail service is linked to user mailboxes, to a site, and to a logo file.

**Mailbox.** The service fields apply to mailboxes that are already connected, and those mailboxes are managed by the methods of the [Mailboxes](../mail/mailbox/index.md) section. The `NAME`, `ACTIVE`, `SERVER`, `PORT`, `ENCRYPTION`, and `LINK` fields changed by the [mailservice.update](./mailservice-update.md) method are propagated to all linked mailboxes. The `ICON` logo and the `SORT` order do not affect mailboxes. If you set an IMAP service to `ACTIVE: N` or delete it using the [mailservice.delete](./mailservice-delete.md) method, the linked mailboxes switch to an empty service of the same site — an active service with `SERVER`, `PORT`, `ENCRYPTION`, and `LINK` left blank. If there is no such service, the mailboxes are disconnected. When mailboxes switch to an empty service, the other fields passed in the same `mailservice.update` call are not propagated to them. For built-in services of other types, deletion always disconnects the linked mailboxes.

**Site.** The service belongs to a site whose identifier is returned in the `SITE_ID` field. The binding cannot be changed via REST.

**File.** The service logo is passed in the `ICON` field either as a file or as the identifier of an existing file.

## Mail Service Fields

The set of service fields is described in the [response object](./mailservice-get.md#mail-service) of the `mailservice.get` method. The [mailservice.list](./mailservice-list.md#mail-services) method returns an array of the same objects. The types and allowed values of writable fields are listed in the parameters of the [mailservice.add](./mailservice-add.md) and [mailservice.update](./mailservice-update.md) methods, and the default values are listed in the parameters of `mailservice.add`. The output cannot be controlled with parameters: `mailservice.list` and `mailservice.fields` are called without parameters, and the section has neither filtering nor pagination. The list of services comes ordered by the `SORT` field, and services with equal values are ordered by `NAME`.

Rules that apply across the section:

- field names are written in uppercase, and boolean values are passed as the strings `Y` and `N`
- `NAME` is required when creating a service, and the `get`, `update`, and `delete` methods require `ID`. The `update` and `delete` methods return `true`
- only the `NAME`, `ACTIVE`, `SERVER`, `PORT`, `ENCRYPTION`, `LINK`, `ICON`, and `SORT` fields can be set when creating and updating a service. All the other fields of the response object, including `ID`, `SITE_ID`, `UPLOAD_OUTGOING`, and the `SMTP_*` fields of the SMTP server, cannot be set via REST
- only an IMAP service can be created via REST: the connection type is set by the system and is not returned in the response
- the [mailservice.update](./mailservice-update.md) method ignores fields with an empty value, so it cannot clear `LINK` or `SERVER`, and `PORT: 0` and `SORT: 0` are not retained. The rule differs in [mailservice.add](./mailservice-add.md): empty `ACTIVE` and `SORT` are replaced with the default values
- the [mailservice.fields](./mailservice-fields.md) method returns field names for interface labels, but not all fields of the object: its response contains neither the SMTP fields nor `UPLOAD_OUTGOING` — see the composition in the [response object](./mailservice-fields.md#fields-map) of this method
- in the response, the `ICON` field comes as a path to the image rather than as a file identifier, so its value cannot be passed back. If the logo is not set and the service name does not match one of the built-in services, the response contains `null`

## Who Can Work with Mail Services

- only an administrator can create, update, and delete a mail service
- any user can retrieve a service by identifier, the list of services, and the field names

## Errors of the Section

The section's own method errors come with HTTP status 400 and a single `ERROR_CORE` code. Only the text in `error_description` differs, so such cases cannot be distinguished by the error code — parse the description. The text depends on the interface language, so do not rely on it as a stable contract. Typical causes:

- insufficient rights
- the required `ID` or `NAME` parameter is not passed
- a service with the specified `ID` is not found
- an invalid field value is passed
- the site has no active services — in this case, [mailservice.list](./mailservice-list.md) returns an error instead of an empty array

Along with the section's own errors, general REST system errors occur: an expired token, a missing `scope`, and exceeded limits. They are described in the article [Error Codes](../../error-codes.md). The list of errors for each method is provided on its page.

## Overview of Methods {#all-methods}

> Scope: [`mailservice`](../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [mailservice.add](./mailservice-add.md) | Creates an email service ||
|| [mailservice.update](./mailservice-update.md) | Updates the parameters of the email service ||
|| [mailservice.get](./mailservice-get.md) | Returns the parameters of the email service by `ID` ||
|| [mailservice.list](./mailservice-list.md) | Returns a list of active email services for the current site ||
|| [mailservice.delete](./mailservice-delete.md) | Deletes an email service ||
|| [mailservice.fields](./mailservice-fields.md) | Returns the names of the fields of the email service ||
|#
