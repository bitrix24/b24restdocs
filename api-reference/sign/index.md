# Signature: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The Bitrix24 KEDO signing service allows you to sign personnel documents with employees using a digital signature. This signature is equivalent to a handwritten one and complies with legal requirements.

> Quick navigation: [all methods and events](#all-methods) 
>
> User documentation: [Bitrix24 e-Signature for HR: service for signing HR documents](https://helpdesk.bitrix24.com/open/20687576/), [Configure access permissions to e-Signature for HR](https://helpdesk.bitrix24.com/open/21579950/)

The methods operate with documents in the KEDO section — Signing with Employees.

These methods can only be executed in the context of authorization of the [application](../../settings/app-installation/index.md).

## Scope Features

**sign.b2e** — used in all methods of the section.

**crm** — used in the methods:
- [sign.b2e.document.send](./sign-b2e-document-send.md)
- [sign.b2e.document.get](./sign-b2e-document-get.md)
- [sign.b2e.company.provider.list](./sign-b2e-company-provider-list.md)

**humanresources.hcmlink** — used in some methods and only when specific fields are provided:

- [sign.b2e.document.send](./sign-b2e-document-send.md):
`company.uuid`, `members.employeeCode`, `members.employeeId`, `responsible.employeeCode`, `responsible.employeeId`.

- [sign.b2e.company.provider.list](./sign-b2e-company-provider-list.md):
`companyUuid`.

## Overview of Methods and Events {#all-methods} 

> Scope: [`sign.b2e`](../scopes/permissions.md)
>
> Who can execute the method: depends on the method

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [sign.b2e.document.send](./sign-b2e-document-send.md) | Sends a document for signing ||
    || [sign.b2e.document.get](./sign-b2e-document-get.md) | Retrieves information about the document and signing participants ||
    || [sign.b2e.company.provider.list](./sign-b2e-company-provider-list.md) | Returns a list of the company's signing providers ||
    || [sign.b2e.personal.tail](./sign-b2e-personal-tail.md) | Returns a list of signed documents for the user ||
    || [sign.b2e.mysafe.tail](./sign-b2e-mysafe-tail.md) | Returns a list of signed documents in the company's safe ||
    |#

- Events

    #| 
    || **Event** | **Triggered** ||
    || [OnSignB2eDocumentStatusChanged](./events/on-sign-b2e-document-status-changed.md) | When the status of the document changes ||
    || [OnSignB2eMemberStatusChanged](./events/on-sign-b2e-member-status-changed.md) | When the status of a signing participant changes ||
    |#

{% endlist %}