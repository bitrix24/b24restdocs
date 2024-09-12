# Universal Methods for Core Elements 

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- We discuss leads, deals, … SPAs, and provide a link to object types.

{% endnote %}

{% endif %}

The methods `crm.item.*` provide capabilities for managing various CRM entities such as leads, deals, contacts, companies, invoices, estimates, and SPA elements. 
They allow you to retrieve fields, add, update, delete, and get lists of entities.

## Features of Working with Field Names

In the database, the names of entity fields are stored in `UPPER_CASE` format. However, when working through REST, we convert them to `camelCase`. For example, the field `ASSIGNED_BY_ID` is transformed into `assignedById`.

Custom field names, in addition to the `_` symbol and letters, can contain numbers. Typically, they are separated, for example, the field `UF_CRM_10_5186744711` is converted to `ufCrm10_5186744711`.

To make the code more readable, underscores between numbers are retained, while others are removed.

Problems arise when letters and numbers in a field are mixed together. For instance, `UF_CRM_10_DIGIT10` is transformed into `ufCrm10Digit10`. When converting back, there is no way to determine whether the original field was named `UF_CRM_10_DIGIT10` or `UF_CRM_10_DIGIT_10`.

To resolve such conflicts, starting from version **CRM 21.1800.0**, a preliminary analysis mechanism for field names was implemented, and if conflicts are detected, the field name is not converted to `camelCase`, but remains as is.

**Additionally, any field names in all methods can be passed in either `UPPER_CASE` or `camelCase`.**

## Linking to Requisites

To manage the linking of requisites, you can use the methods [crm.requisite.link.*](../requisites/links/index.md).

## Starting from crm 21.800.0

Support for parent fields when working with CRM entities is introduced.

Each field has the code `parentId + {parentEntityTypeId}`.

- The `fields` method in the field list will return information about fields with parents.
- The `get` method will return the values of parent fields.
- The `list` method will filter, sort, and include the values of parent fields in the selection.
- The `add` and `update` methods will support changes to the values of these fields.

## REST Methods

- [{#T}](crm-item-fields.md)
- [{#T}](crm-item-add.md)
- [{#T}](crm-item-update.md)
- [{#T}](crm-item-delete.md)
- [{#T}](crm-item-list.md)

## Events on SPA Elements

- [{#T}](events/on-crm-dynamic-item-add.md)
- [{#T}](events/on-crm-dynamic-item-update.md)
- [{#T}](events/on-crm-dynamic-item-delete.md)