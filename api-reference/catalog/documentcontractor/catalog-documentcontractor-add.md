# Add CRM entity (Contact/Company) binding to document catalog.documentcontractor.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of error
- no examples in other languages
- create a link from [List](.) to Constants CRM
- create a link [crm.category.list](.)
- create a link [crm.item.list](.)
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.documentcontractor.add(fields)
```

This method adds a binding of a CRM entity (Contact/Company) to a document. The response includes a list of binding fields, similar to the method [`getFields`](catalog-documentcontractor-get-fields.md). The list returns the `id` of the binding for further work with the binding.

### Cases when errors may occur while adding a binding to the document:

- if there is no access to inventory management, no access to view stock receipt documents, or no access to edit the stock receipt document (adding/removing a binding is considered editing the document);
- if the document is not found;
- if the document type is incorrect (binding to vendors is only relevant for documents of type **Stock Receipt**);
- if the document has already been processed (the document cannot be edited after it has been processed);
- if the entity type is incorrect (only values 3 or 4 are accepted in the **entityTypeId** field);
- when trying to bind a vendor Company to a stock receipt document when a vendor Company is already bound to that document (multiple Contacts can be bound, but only one Company is allowed).

## Parameters

#|
|| **Parameter** |  **Description** ||
|| **fields** 
[`object`](../../data-types.md) | Fields **documentId**, **entityTypeId**, and **entityId**, corresponding to the available list of fields [`fields`](catalog-documentcontractor-get-fields.md). ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    BX.callMethod(
        'catalog.documentcontractor.add',
        {
            fields: {
                documentId: 11,
                entityTypeId: 3,
                entityId: 21,
            },
        },
        function(result) {
            if (result.error())
                console.error(result.error().ex);
            else
                console.log(result.data());
        }
    );
    ```

{% endlist %}

The document identifier **documentId** is known. Let's take a closer look at where to get **entityTypeId** and **entityId**:

- **entityTypeId** – id of the CRM entity type. [List](.) of these entity types is fixed (for Contact it is `3`, for Company `4`).
- **entityId** – id of the CRM entity (Vendor Contacts or Vendor Companies). To get the list of the required CRM entity ids, two steps are needed:
    
    - To obtain the list of entities, use the method [crm.category.list](.), passing **entityTypeId** (3 or 4) and filtering by [category code](*category_code_key) (for Contact `CATALOG_CONTRACTOR_CONTACT`, for Company `CATALOG_CONTRACTOR_COMPANY`):
  
    ```js
    BX.callMethod(
        'crm.category.list',
        {
            entityTypeId: 3,
            filter: {
                code: 'CATALOG_CONTRACTOR_CONTACT',
            }
        },
        console.log(result.data())
    );
    ```

    The response will return data about the category. For the second step, **categoryId** is needed – the id of the category (it may coincide with **entityTypeId**).

   - Next, obtain the list of CRM entities using the method [crm.item.list](.), passing the entity id **entityTypeId** and filtering by the previously obtained category id **categoryId**:

    ```js
    BX.callMethod(
        'crm.item.list',
        {
            entityTypeId: 3, // id of the CRM entity type Contact
            select: ['ID', 'NAME', 'LAST_NAME', 'CATEGORY_ID'], // fields to display, optional parameter
            filter: {
                categoryId: 3, // id of the CRM entity Contact vendor category, obtained from crm.category.list
            },
        },
        console.log(result.data())
    );
    ```
    
    A list of Vendor Contacts will be returned, which can be used in bindings (the ids of Contacts are needed for **entityId**).

    Similarly, to obtain the list of Vendor Companies:

    ```js
    BX.callMethod(
        'crm.item.list',
        {
            entityTypeId: 4, // id of the CRM entity type Company
            select: ['ID', 'TITLE', 'CATEGORY_ID'], // fields to display, optional parameter
            filter: {
                categoryId: 4, // id of the CRM entity Vendor Company category, obtained from crm.category.list
            },
        },
        console.log(result.data())
    );
    ```

{% include [Note on examples](../../../_includes/examples.md) %}

[*category_code_key]: Filtering by category in the method **crm.category.list** is available since version **crm 23.0.0**