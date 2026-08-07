# e-Signature Integration with HR Systems: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In Bitrix24 e-Signature, you can configure data exchange with an external HR system. The integration helps automate HR document signing, reduce manual data entry, and lower the risk of errors.

With Bitrix24 e-Signature and HR system integration, you can:

- link Bitrix24 companies to companies in an HR system
- synchronize employees and map them to Bitrix24 users
- transfer HR system field values to fill e-Signature documents
- update synchronization jobs and track their status

The methods can only be executed in the context of authorization of the [application](../../../settings/app-installation/index.md). For most methods, the user must be an administrator.

> Quick navigation: [all methods](#all-methods)

## How sign.b2e and humanresources.hcmlink Are Related

`sign.b2e` manages e-Signature documents: it sends a document for signing, retrieves a document, returns signing providers, and returns lists of signed documents.

`humanresources.hcmlink` supports e-Signature integration with an HR system. These methods create a link to a company in the HR system, upload employees, return mapped employees, and transfer field values that are later used to fill e-Signature documents.

## Main Data Exchange Scenario

**Connecting a company.** The application adds a company from an HR system using [humanresources.hcmlink.company.add](./humanresources-hcmlink-company-add.md). The `fields.crmCompanyId` parameter passes the CRM company ID, and `fields.fields` passes HR system fields that will be available for filling e-Signature documents.

**Synchronizing employees.** When synchronization starts in the interface, the application receives the `OnHumanResourcesHcmLinkEmployeeListRequested` event with the job ID `jobId`, company code `company`, and request date. After that, the application transfers the employee list using [humanresources.hcmlink.employee.set](./humanresources-hcmlink-employee-set.md) and completes the job using [humanresources.hcmlink.job.update](./humanresources-hcmlink-job-update.md).

**Mapping users.** After Bitrix24 users are mapped to individuals from the HR system, the application receives the `OnHumanResourcesHcmLinkEmployeeListMapped` event. Use this event to request changed mappings with [humanresources.hcmlink.employee.list](./humanresources-hcmlink-employee-list.md). Pass the `updatedAt` parameter to retrieve only changes, not the entire employee list.

**Requesting values for documents.** When an e-Signature document is sent, Bitrix24 requests HR system field values using the `OnHumanResourcesHcmLinkFieldValueRequested` event. The event contains `jobId`, the company code, the employee list, and the field list. The application transfers values using [humanresources.hcmlink.field.value.set](./humanresources-hcmlink-field-value-set.md), then updates the job using [humanresources.hcmlink.job.update](./humanresources-hcmlink-job-update.md).

## Employee IDs

The exchange uses two employee identifiers from the HR system:

- `person` — the individual code in the HR system
- `employee` — the employee code in the HR system

One individual can be linked to multiple employees in one company, for example if the person has several positions. Bitrix24 maps a user to `person`, and requests field values for documents by `employee`.

## Related e-Signature Scenarios

**Payroll and Vacation.** For payslip and vacation balance pages, the application registers the `HCMLINK_SALARY_VACATION` embedding. The [humanresources.hcmlink.company.user.list](./humanresources-hcmlink-company-user-list.md) method returns companies where the current user is mapped to an individual from the HR system.

**PIN and payroll data.** PIN requests and payroll and vacation data requests use the `OnHumanResourcesHcmLinkPinRequested` and `OnHumanResourcesHcmLinkSalaryVacationRequested` events. Responses for these jobs are transferred using [humanresources.hcmlink.field.value.set](./humanresources-hcmlink-field-value-set.md).

## Connection with Other Objects

**CRM company.** The `humanresources.hcmlink.company.add` method links a company from an HR system to a CRM company through the `fields.crmCompanyId` parameter. You can get the company ID using [crm.item.list](../../crm/universal/crm-item-list.md) with the `entityTypeId = 4` parameter and the `isMyCompany = Y` filter. In the interface, these companies are located in `CRM > More > Settings > My Company Details`. This ID is used in e-Signature methods when a document is sent on behalf of a company.

**Bitrix24 employee.** The `humanresources.hcmlink.employee.set` method links an employee from an HR system to a Bitrix24 user through `data[].userId`. After mapping, e-Signature documents can be sent by `employeeId` or `employeeCode` in [sign.b2e.document.send](../sign-b2e-document-send.md).

**HR system fields.** The list of available fields is transferred when a company is added or updated. The codes of these fields are used in value request events and in [humanresources.hcmlink.field.value.set](./humanresources-hcmlink-field-value-set.md).

## Overview of Methods {#all-methods}

> Scope: [`humanresources.hcmlink`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

### Companies

#|
|| **Method** | **Description** ||
|| [humanresources.hcmlink.company.add](./humanresources-hcmlink-company-add.md) | Adds a company from an HR system ||
|| [humanresources.hcmlink.company.update](./humanresources-hcmlink-company-update.md) | Updates a company from an HR system and the list of its fields ||
|| [humanresources.hcmlink.company.list](./humanresources-hcmlink-company-list.md) | Retrieves a list of companies from an HR system ||
|| [humanresources.hcmlink.company.user.list](./humanresources-hcmlink-company-user-list.md) | Retrieves companies from an HR system linked to the current user ||
|| [humanresources.hcmlink.company.delete](./humanresources-hcmlink-company-delete.md) | Deletes a company from the HCM Link integration ||
|#

### Employees

#|
|| **Method** | **Description** ||
|| [humanresources.hcmlink.employee.set](./humanresources-hcmlink-employee-set.md) | Transfers a list of employees from an HR system ||
|| [humanresources.hcmlink.employee.list](./humanresources-hcmlink-employee-list.md) | Retrieves a list of mapped employees from the HR system and Bitrix24 ||
|#

### Field Values

#|
|| **Method** | **Description** ||
|| [humanresources.hcmlink.field.value.set](./humanresources-hcmlink-field-value-set.md) | Transfers HR system field values for employees ||
|#

### Synchronization Jobs

#|
|| **Method** | **Description** ||
|| [humanresources.hcmlink.job.update](./humanresources-hcmlink-job-update.md) | Updates a synchronization job ||
|| [humanresources.hcmlink.job.status.get](./humanresources-hcmlink-job-status-get.md) | Checks whether a synchronization job is active ||
|#
