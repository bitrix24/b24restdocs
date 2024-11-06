# Universal List Fields: Overview of Methods

Fields define the format and type of information that can be stored in list items: one field for the title, another for the date, and a third for the text description. Each field has a specified type: String, Creation Date, and HTML/Text. A complete list of field types is described in the `FIELDS` parameter of the [lists.field.add](./lists-field-add.md) method. Through field types, universal lists are integrated with the CRM and Drive modules.

{% note warning "" %}

The type is set when creating the field and cannot be changed.

{% endnote %}

> Quick navigation: [all methods](#all-methods) 
>
> User documentation: [creating list fields](https://helpdesk.bitrix24.com/open/16702950/)

## Linking to CRM Entities

The "Link to CRM Entities" field type connects list items with CRM objects: [leads](../../crm/leads/index.md), [deals](../../crm/deals/index.md), [SPAs](../../crm/universal/index.md), [contacts](../../crm/contacts/index.md), and [companies](../../crm/companies/index.md). In list items, this field is displayed as a link to the CRM object, and a tab with the list name is automatically created in the CRM object card.

When filling out the list, consider the link to CRM objects: leads, deals, SPAs, contacts, and companies.
- If the field settings allow linking to only one object, fill in the field value with the [object identifier](../../crm/data-types.md#object_type) without the symbolic prefix. For example, `3` — contact. Otherwise, the field will be displayed but will not be exported to Excel.
- If linking to multiple objects is allowed, fill in the field value with the [identifier with prefix](../../crm/data-types.md#object_type). For example, `C_3` — contact.

## Documents and Files

Universal lists support two types of fields for working with files.

**File (Drive).** This field type is associated with Bitrix24 Drive and stores the identifier of the Drive object. Work with the file should be done using its identifier through the [disk.*](../../disk/index.md) methods.

**File.** This field type allows uploading and storing files directly in the list item. It does not have a Drive object identifier. Using Drive methods is not possible.

## Overview of Methods {#all-methods}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [lists.field.add](./lists-field-add.md) | Creates a list field ||
|| [lists.field.delete](./lists-field-delete.md) | Deletes a list field ||
|| [lists.field.get](./lists-field-get.md) | Returns field data ||
|| [lists.field.type.get](./lists-field-type-get.md) | Returns available field types for the specified list ||
|| [lists.field.update](./lists-field-update.md) | Updates a list field ||
|#