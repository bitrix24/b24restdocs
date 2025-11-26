# Universal List Elements: Overview of Methods

Each list element consists of fields. You can add new fields and configure them using the methods [lists.field.*](../fields/index.md).

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [How to add list elements](https://helpdesk.bitrix24.com/open/16701764/)

## Connection with Other Objects

**Universal Lists.** Elements are created within lists. To add an element to a list, specify the parameters `IBLOCK_TYPE_ID`, `IBLOCK_ID`, and `IBLOCK_CODE`. Values can be obtained using the method [lists.get](../lists/lists-get.md).

**List Sections.** An element can be placed in a section using the parameter `IBLOCK_SECTION_ID`. The section identifier can be obtained using the method [lists.section.get](../sections/lists-section-get.md).

**Elements from Other Lists.** The field type Link to elements connects elements from different lists. In such a field, you can select another list and specify its elements as values. The `ID` of the selected elements is stored within the field, while their names are displayed.

For example, if you already have a list called Sizes with elements: XS, S, M, L, then in the Products list, you create a field of type Link to elements and specify the Sizes list. Now, when you add a product, you don't enter sizes manually but select the required elements from the list — for instance, S and M.

To obtain the list identifier, use the method [lists.get](../lists/lists-get.md). The element identifier can be found using the method [lists.element.get](./lists-element-add.md).

**CRM Objects.** The field type Link to CRM elements connects list elements with CRM objects: [leads](../../crm/leads/index.md), [deals](../../crm/deals/index.md), [SPAs](../../crm/universal/index.md), [contacts](../../crm/contacts/index.md), and [companies](../../crm/companies/index.md). In list elements, this field is displayed as a link to the CRM object, and in the CRM card itself, a tab with the list name appears.

The format of the field value depends on the settings.

- If the field settings allow linking to only one object, fill the field value with the [object identifier](../../crm/data-types.md#object_type) without a symbolic prefix. For example, `3` — contact. Otherwise, the field will be displayed but not exported to Excel.

- If linking to multiple objects is allowed, fill the field value with the [identifier with a prefix](../../crm/data-types.md#object_type). For example, `C_3` — contact.

**Users.** To work with users in list elements, fields of type Link to employee, Created by, and Modified by are used. The values in the Created by and Modified by fields are automatically filled by the system. For fields of type Link to employee, data must be provided manually.

The user identifier can be obtained using the method [user.get](../../user/user-get.md).

**Drive.** The field type File (Drive) connects list elements with the Drive. To add a file to the element's field:

- upload the file to the Drive using the methods [disk.folder.uploadfile](../../disk/folder/disk-folder-upload-file.md) or [disk.storage.uploadfile](../../disk/storage/disk-storage-upload-file.md),

- obtain the `ID` of the uploaded file,

- attach the file using the methods [lists.element.add](./lists-element-add.md) or [lists.element.update](./lists-element-update.md).

## Overview of Methods {#all-methods}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [lists.element.add](./lists-element-add.md) | Creates a list element ||
|| [lists.element.update](./lists-element-update.md) | Updates a list element ||
|| [lists.element.get](./lists-element-get.md) | Returns an element or a list of elements ||
|| [lists.element.delete](./lists-element-delete.md) | Deletes a list element ||
|| [lists.element.get.file.url](./lists-element-get-file-url.md) | Returns the file path ||
|#