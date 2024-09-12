# Methods for Working with CRM Vendors

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

New vendors are CRM entities. The mechanism for linking entities to inventory documents is used. Vendors are relevant only for incoming documents.

The service can only be used when working with vendors through CRM. Multiple vendor contacts (CRM entity Contact) can be attached to one incoming document, but no more than one vendor company (CRM entity Company).

The service is available starting from version **catalog 23.0.0** and **rest 23.100.0**.

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can perform the method: any user

Service methods for working with CRM vendors:

#|
|| **Method** | **Description** ||
|| [catalog.documentcontractor.add](./catalog-documentcontractor-add.md) | This method adds a link of a CRM entity (Contact/Company) to a document. ||
|| [catalog.documentcontractor.delete](./catalog-documentcontractor-delete.md) | This method removes the link of a CRM entity (Contact/Company) to a document by the link identifier. ||
|| [catalog.documentcontractor.getFields](./catalog-documentcontractor-get-fields.md) | This method returns an array of fields with links of CRM vendors to inventory documents. ||
|| [catalog.documentcontractor.list](./catalog-documentcontractor-list.md) | This method retrieves a list of links of CRM entities (Contact/Company) to documents based on a filter. ||
|#