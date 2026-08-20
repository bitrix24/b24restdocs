# e-Signature: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Bitrix24 e-Signature allows you to sign HR documents with employees using a digital signature. This signature is equivalent to a handwritten one and complies with legal requirements.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation:
> - [Bitrix24 e-Signature for HR: service for signing HR documents](https://helpdesk.bitrix24.com/open/20687576/)
> - [Configure access permissions to e-Signature for HR](https://helpdesk.bitrix24.com/open/21579950/)

The `sign.b2e.*` methods work with documents in e-Signature. Use them to send a document for signing, retrieve document data, retrieve a list of signing providers, and retrieve lists of signed documents.

The `humanresources.hcmlink.*` methods manage e-Signature integration data with HR systems through HCM Link: companies, employees, mappings, and field values.

These methods can only be executed in the context of authorization of the [application](../../settings/app-installation/index.md).

## How to Select a Method Group

#|
|| **If You Need To** | **Use** ||
|| Send documents for signing and retrieve signing information | `sign.b2e.*` methods ||
|| Track changes in document and signing participant statuses | `OnSignB2e*` events ||
|| Transfer data from an HR system to fill e-Signature documents | [humanresources.hcmlink.*](./hcm-link/index.md) methods ||
|#

## Link to Other Objects

**CRM company.** The e-Signature document is sent on behalf of a company. In the `sign.b2e.document.send` and `sign.b2e.company.provider.list` methods, you can specify the company using `company.crmId` or `companyCrmId`. This is the identifier of the CRM company connected as "my company". You can retrieve it using the [crm.item.list](../crm/universal/crm-item-list.md) method with the `entityTypeId = 4` parameter and the `isMyCompany = Y` filter.

**Employee and user.** Signing participants and the responsible person are linked to Bitrix24 users through `userId`. If the company is connected to HCM Link, you can pass the employee's `employeeId` or `employeeCode` from the HR system instead of `userId`.

**HCM Link.** The integration with HR systems transfers companies, employees, and field values for e-Signature documents. If a document is sent using HCM Link data, the `sign.b2e.*` methods are linked to the [humanresources.hcmlink.*](./hcm-link/index.md) methods.

**Document file.** The `sign.b2e.document.send` method accepts a PDF file in the `files[].fileContent` parameter, encoded in Base64. After signing, the file can be retrieved from the user's signed document lists or from the company safe. If the document is linked to HCM Link, signed file data can be retrieved using the [sign.b2e.hcmlink.document.get](./sign-b2e-hcmlink-document-get.md) method by the signing participant identifier.

**Signing statuses.** The `OnSignB2eDocumentStatusChanged` and `OnSignB2eMemberStatusChanged` events report changes in document and participant statuses. The `OnSignHcmLinkB2eDocumentSigned` event reports that a document linked to HCM Link has been signed. Document details can be retrieved using the [sign.b2e.document.get](./sign-b2e-document-get.md) method by `uid`.

## Scope Features

**sign.b2e** — used in methods for working with e-Signature documents and signing events.

**crm** — used in the methods:
- [sign.b2e.document.send](./sign-b2e-document-send.md)
- [sign.b2e.document.get](./sign-b2e-document-get.md)
- [sign.b2e.company.provider.list](./sign-b2e-company-provider-list.md)

**humanresources.hcmlink** — used:

- in [sign.b2e.document.send](./sign-b2e-document-send.md) and [sign.b2e.company.provider.list](./sign-b2e-company-provider-list.md) if the application passes HCM Link data: `company.uuid`, `members.employeeCode`, `members.employeeId`, `responsible.employeeCode`, `responsible.employeeId`, `companyUuid`
- in e-Signature integration methods with HR systems. The detailed scenario is described in [e-Signature Integration with HR Systems](./hcm-link/index.md)

## Overview of Methods and Events {#all-methods}

### e-Signature Documents

> Scope: [`sign.b2e`](../scopes/permissions.md)
>
> Who can execute the method: depends on the method

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [sign.b2e.document.send](./sign-b2e-document-send.md) | Sends a document for signing ||
    || [sign.b2e.document.get](./sign-b2e-document-get.md) | Retrieves information about the document and signing participants ||
    || [sign.b2e.hcmlink.document.get](./sign-b2e-hcmlink-document-get.md) | Retrieves data of a signed document linked to HCM Link ||
    || [sign.b2e.company.provider.list](./sign-b2e-company-provider-list.md) | Returns a list of the company's signing providers ||
    || [sign.b2e.personal.tail](./sign-b2e-personal-tail.md) | Returns a list of signed documents for the user ||
    || [sign.b2e.mysafe.tail](./sign-b2e-mysafe-tail.md) | Returns a list of signed documents in the company's safe ||
    |#

- Events

    #| 
    || **Event** | **Triggered** ||
    || [OnSignB2eDocumentStatusChanged](./events/on-sign-b2e-document-status-changed.md) | When the status of the document changes ||
    || [OnSignB2eMemberStatusChanged](./events/on-sign-b2e-member-status-changed.md) | When the status of a signing participant changes ||
    || [OnSignHcmLinkB2eDocumentSigned](./events/on-sign-hcm-link-b2e-document-signed.md) | When a document linked to HCM Link is signed ||
    |#

{% endlist %}

### Integration with HR Systems

> Scope: [`humanresources.hcmlink`](../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [humanresources.hcmlink.company.add](./hcm-link/humanresources-hcmlink-company-add.md) | Adds a company from an HR system ||
|| [humanresources.hcmlink.company.update](./hcm-link/humanresources-hcmlink-company-update.md) | Updates a company from an HR system and the list of its fields ||
|| [humanresources.hcmlink.company.list](./hcm-link/humanresources-hcmlink-company-list.md) | Retrieves a list of companies from an HR system ||
|| [humanresources.hcmlink.company.user.list](./hcm-link/humanresources-hcmlink-company-user-list.md) | Retrieves companies from an HR system linked to the current user ||
|| [humanresources.hcmlink.company.delete](./hcm-link/humanresources-hcmlink-company-delete.md) | Deletes a company from the HCM Link integration ||
|| [humanresources.hcmlink.employee.set](./hcm-link/humanresources-hcmlink-employee-set.md) | Transfers a list of employees from an HR system ||
|| [humanresources.hcmlink.employee.list](./hcm-link/humanresources-hcmlink-employee-list.md) | Retrieves a list of mapped employees from the HR system and Bitrix24 ||
|| [humanresources.hcmlink.field.value.set](./hcm-link/humanresources-hcmlink-field-value-set.md) | Transfers HR system field values for employees ||
|| [humanresources.hcmlink.job.update](./hcm-link/humanresources-hcmlink-job-update.md) | Updates a synchronization job ||
|| [humanresources.hcmlink.job.status.get](./hcm-link/humanresources-hcmlink-job-status-get.md) | Checks whether a synchronization job is active ||
|#
