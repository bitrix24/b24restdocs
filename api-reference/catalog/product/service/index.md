# Services: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Services are intangible goods: consultations, work, or actions. They can be used in CRM entities and be associated with physical products.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Services in CRM](https://helpdesk.bitrix24.com/open/16680094/)

## Relationship with Other Objects

**Product Catalog.** A service must be linked to a specific product catalog. You can obtain the identifiers of available product catalogs using the method [catalog.catalog.list](../../catalog/catalog-catalog-list.md).

**Sections of the Product Catalog.** Services are usually distributed across sections. To create and manage sections, use the [catalog.section.*](../../section/index.md) method group.

**Images.** A service can contain images: for announcement, detailed, additional. To add images, use the methods [catalog.productImage.*](../../product-image/index.md), and to download, use the method [catalog.product.service.download](./catalog-product-service-download.md).

**Units of Measurement.** For a service, select a unit of measurement, for example hours for consultations. You can add or change the unit of measurement using the [catalog.measure.*](../../measure/index.md) methods.

**VAT.** The VAT rate can be set for each service individually. You can work with rates through the methods [catalog.vat.*](../../vat/index.md).

**User.** Each service stores the identifiers of users who created and modified it. User information can be obtained using [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md).

**Product and Variation Properties.** Services have properties that distinguish them from one another. These can include the type of service, execution time, or service status. You can work with properties using the methods [catalog.productProperty.*](../../product-property/index.md).

**CRM.** Services are connected to CRM in the following ways:

- A service can be added to the list of products for a [lead](../../../crm/leads/index.md), [deal](../../../crm/deals/index.md), [invoice](../../../crm/universal/invoice.md), [SPA](../../../crm/universal/index.md), and [estimate](../../../crm/quote/index.md).

- [Leads](../../../crm/leads/index.md), [deals](../../../crm/deals/index.md), [SPAs](../../../crm/universal/index.md), [invoices](../../../crm/universal/invoice.md), [contacts](../../../crm/contacts/index.md), and [companies](../../../crm/companies/index.md) can be specified in services using the property type "Link to CRM entities."

**Order Basket.** A service can be added, modified, or removed from the basket using the [sale.basketitem.*](../../../sale/basket-item/index.md) method group.

## How to Start

1. Get the trade catalog identifier using [catalog.catalog.list](../../catalog/catalog-catalog-list.md)
2. If services need to be distributed across sections, create sections using [catalog.section.*](../../section/index.md)
3. Create a service using [catalog.product.service.add](./catalog-product-service-add.md)
4. Add images using [catalog.productImage.add](../../product-image/catalog-product-image-add.md), if needed
5. Set the service price using [catalog.price.*](../../price/index.md)

## Services in REST 3.0

In [REST 3.0](../../../rest-v3.md), services have methods for adding, updating, retrieving, deleting, and downloading files, as well as the `catalog.product.service.field.list` and `catalog.product.service.field.get` methods for retrieving the field schema. Methods available in both API versions are marked `v 3.0` in the table. The general specifics of the new version product methods are described in the [products overview](../index.md).

The [catalog.product.service.add](./catalog-product-service-add-rest-v3.md) method assigns type `7` to the service itself, so you do not need to pass the type.

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the methods:
> - administrator — for methods of the previous API version
> - a user with the "View product catalog", "View catalog products", or "Edit selling price" access permission and permissions for the information block — for retrieving services in REST 3.0. Adding, updating, and deleting also require the "Create product", "Edit product", or "Delete product" access permission

#|
|| **Method** | **Description** ||
|| [catalog.product.service.add](./catalog-product-service-add.md) | Adds a service ||
|| [catalog.product.service.add](./catalog-product-service-add-rest-v3.md) | Adds a service v 3.0 ||
|| [catalog.product.service.update](./catalog-product-service-update.md) | Updates the service fields ||
|| [catalog.product.service.update](./catalog-product-service-update-rest-v3.md) | Updates the service fields v 3.0 ||
|| [catalog.product.service.get](./catalog-product-service-get.md) | Returns the values of service fields by identifier ||
|| [catalog.product.service.get](./catalog-product-service-get-rest-v3.md) | Returns the values of service fields by identifier v 3.0 ||
|| [catalog.product.service.list](./catalog-product-service-list.md) | Returns a list of services by filter ||
|| [catalog.product.service.list](./catalog-product-service-list-rest-v3.md) | Returns a list of services by filter v 3.0 ||
|| [catalog.product.service.download](./catalog-product-service-download.md) | Downloads service files based on the provided parameters ||
|| [catalog.product.service.download](./catalog-product-service-download-rest-v3.md) | Downloads service files based on the provided parameters v 3.0 ||
|| [catalog.product.service.delete](./catalog-product-service-delete.md) | Deletes a service ||
|| [catalog.product.service.delete](./catalog-product-service-delete-rest-v3.md) | Deletes a service v 3.0 ||
|| [catalog.product.service.getFieldsByFilter](./catalog-product-service-get-fields-by-filter.md) | Returns service fields by filter ||
|| [catalog.product.service.field.list](./catalog-product-service-field-list.md) | Returns a list of service fields v 3.0 ||
|| [catalog.product.service.field.get](./catalog-product-service-field-get.md) | Returns the description of a service field ||
|#
