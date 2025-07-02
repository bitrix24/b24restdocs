# Services: Overview of Methods

Services are intangible goods: consultations, work, or actions. They can be used in CRM entities and be associated with physical products.

> Quick navigation: [all methods](#all-methods) 
> 
> User documentation: [Services in CRM](https://helpdesk.bitrix24.com/open/16680094/)

## Connection of Services with Other Entities

**Product Catalog.** A service must be linked to a specific product catalog. You can obtain the identifiers of available product catalogs using the method [catalog.catalog.list](../../catalog/catalog-catalog-list.md).

**Sections of the Product Catalog.** Services are typically distributed across sections. To create and manage sections, use the group of methods [catalog.section.*](../../section/index.md).

**Images.** A service can contain images: for announcement, detailed view, additional. To add images, use the methods [catalog.productImage.*](../../product-image/index.md), and to download, use the method [catalog.product.service.download](./catalog-product-service-download.md).

**Units of Measurement.** A unit of measurement is chosen for the service, for example, hours for consultations. You can add or change the unit of measurement using the methods [catalog.measure.*](../../measure/index.md).

**VAT.** The VAT rate can be set for each service individually. You can work with rates through the methods [catalog.vat.*](../../vat/index.md).

**User.** Each service stores the identifiers of users who created and modified it. Information about the user can be obtained using the methods [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md).

**Product and Variation Properties.** Services have properties that distinguish them from one another. These can include the type of service, completion time, or service status. You can work with properties using the methods [catalog.productProperty.*](../../product-property/index.md).

**CRM.** Services are connected to CRM in the following ways:

- A service can be added to the list of products in a [lead](../../../crm/leads/index.md), [deal](../../../crm/deals/index.md), [invoice](../../../crm/universal/invoice.md), [SPA](../../../crm/universal/index.md), and [estimate](../../../crm/quote/index.md).

- [Leads](../../../crm/leads/index.md), [deals](../../../crm/deals/index.md), [SPAs](../../../crm/universal/index.md), [invoices](../../../crm/universal/invoice.md), [contacts](../../../crm/contacts/index.md), and [companies](../../../crm/companies/index.md) can be specified in services using the property type "Link to CRM entities."

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

#|
|| **Method** | **Description** ||
|| [catalog.product.service.add](./catalog-product-service-add.md) | Adds a service ||
|| [catalog.product.service.update](./catalog-product-service-update.md) | Updates service fields ||
|| [catalog.product.service.get](./catalog-product-service-get.md) | Returns service field values by identifier ||
|| [catalog.product.service.list](./catalog-product-service-list.md) | Returns a list of services by filter ||
|| [catalog.product.service.download](./catalog-product-service-download.md) | Downloads service files by provided parameters ||
|| [catalog.product.service.delete](./catalog-product-service-delete.md) | Deletes a service ||
|| [catalog.product.service.getFieldsByFilter](./catalog-product-service-get-fields-by-filter.md) | Returns service fields by filter ||
|#