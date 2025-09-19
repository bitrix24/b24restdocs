# Invoices: Overview of Methods

An invoice is the final stage of a deal. It is created when all discussions are complete and the terms of the agreement are agreed upon. Multiple invoices can be created for different products and services within a single deal.

An invoice can be generated from a template and sent to the client as a document. In the invoice detail form, you can:

- Manage the sales process of a product or service
- Track the stages of working with the invoice
- Accept online payments

> Quick navigation: [all methods and events](#all-methods) 
>
> User documentation: [New invoices in CRM](https://helpdesk.bitrix24.com/open/14809588/)

## Linking Invoices with Other CRM Objects

**Deal.** Pass the deal ID in the `parentId2` parameter to link the new invoice with the deal.

**Estimate.** Pass the estimate ID in the `parentId7` parameter to link the new invoice with the estimate.

**Client.** This field in the invoice detail form consists of the associated company and contacts. All activities related to calls, e-mails, and chats with the contact or company will be saved in the invoice detail form. There can be one company in the field, and it is referenced through the invoice field `companyId`. Multiple contacts can be specified, and interactions with them are managed through the `contactIds` field; pass an array of contact IDs in this field.

**Products.** Adding, modifying, and deleting product items in invoices can be done through the group of methods [crm.item.productrow.*](./product-rows/index.md).

**Payments.** Adding, modifying, and deleting payment documents in invoices can be done through the group of methods [crm.item.payment.*](./payment/index.md).

**Your Company Details.** Specify your company ID in the `mycompanyId` field so that its details are automatically used in documents. You can obtain your company ID using the method [crm.item.list](./crm-item-list.md) for the companies object with a filter on the `isMyCompany` field.

```JavaScript
BX24.callMethod(
        'crm.item.list',
        {
            entityTypeId: 4,
            filter: {
                "isMyCompany": "Y",
            },
        },
        (result) => {
            if (result.error())
            {
                console.error(result.error());
                return;
            }
            console.info(result.data());
        },
    );
```

{% note tip "User Documentation" %}

- [How to accept payments from clients and work with receipts in Bitrix24](https://helpdesk.bitrix24.com/open/23570150/)
- [How to add products to deals, leads, and estimates](https://helpdesk.bitrix24.com/open/14303190/)
- [How to use your company details](https://helpdesk.bitrix24.com/open/16059544/)

{% endnote %}

## Invoice Detail Form

The main workspace in invoices is the General tab of the detail form. It consists of two parts:

- The left part contains fields with information. If the system fields are insufficient, you can create your own custom fields. They allow you to store information in various data formats: string, number, link, address, and others. To create, modify, retrieve, or delete custom fields for invoices, use the group of methods [userfieldconfig.*](./userfieldconfig/userfieldconfig/userfieldconfig-add.md).

- The right part contains the invoice timeline. In it, you can create, edit, filter, and delete CRM activities — the group of methods [crm.activity.*](../timeline/activities/index.md), and timeline records — the group of methods [crm.timeline.*](../timeline/index.md).

The parameters of the invoice detail form can be managed through the group of methods [crm.item.details.configuration.*](./item-details-configuration/index.md).

{% note tip "User Documentation" %}

- [CRM Detail Form: Features and Settings](https://helpdesk.bitrix24.com/open/22879716/)
- [System Fields in CRM](https://helpdesk.bitrix24.com/open/18529390/)
- [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)
- [Timeline in CRM Entity](https://helpdesk.bitrix24.com/open/16767378/)

{% endnote %}

## Widgets

You can embed an application into the invoice detail form. This allows you to use the application without leaving the invoice detail form.

There are two embedding scenarios:

- Use special [embedding locations](../../widgets/crm/index.md). For example, by creating your own tab.
- Create a [custom field](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md) where the interface of your application will be loaded.

### Embedding Locations for New Invoices

- [`CRM_SMART_INVOICE_DETAIL_TAB`](../../widgets/crm/detail-tab.md) — a tab in the detailed view of the CRM entity

- [`CRM_SMART_INVOICE_DETAIL_ACTIVITY`](../../widgets/crm/detail-activity.md) — a button above the timeline of the detail form

- [`CRM_SMART_INVOICE_DETAIL_TOOLBAR`](../../widgets/crm/detail-toolbar.md) — an item in the dropdown menu of the top button in the detail form

- [`CRM_SMART_INVOICE_DOCUMENTGENERATOR_BUTTON`](../../widgets/crm/document-generator-button.md) — an item in the dropdown menu of the document generator

- [`CRM_SMART_INVOICE_LIST_MENU`](../../widgets/crm/index.md) — an item in the context menu in the list of entities

- [`CRM_SMART_INVOICE_LIST_TOOLBAR`](../../widgets/crm/list-toolbar.md) — an item in the dropdown menu above the list of entities

- [`CRM_SMART_INVOICE_TIMELINE_MENU`](../../widgets/crm/activity-timeline-menu.md) — an item in the context menu of an activity in the detail form

- [`CRM_SMART_INVOICE_ROBOT_DESIGNER_TOOLBAR`](../../widgets/crm/robot-designer-toolbar.md) — an item in the dropdown menu of the top button of the robot designer

{% note tip "Typical use-cases and scenarios" %}

- [Widget Embedding Mechanism](../../widgets/index.md)
- [Embed a Widget in the CRM Detail Form](../../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)

{% endnote %}

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: depending on the method

### Main

CRM Object Identifier **entityTypeId** — `31`

{% list tabs %}

- Methods

    #|
    || [crm.item.add](./crm-item-add.md) | Creates a new CRM entity ||
    || [crm.item.update](./crm-item-update.md) | Updates an entity ||
    || [crm.item.get](./crm-item-get.md) | Returns an entity by Id ||
    || [crm.item.list](./crm-item-list.md) | Returns a list of entities by filter ||
    || [crm.item.delete](./crm-item-delete.md) | Deletes an entity ||
    || [crm.item.fields](./crm-item-fields.md) | Returns the fields of an entity ||
    |#

- Events

    #|
    || [onCrmDynamicItemAdd](./events/on-crm-dynamic-item-add.md) | When a custom type CRM object is created ||
    || [onCrmDynamicItemDelete](./events/on-crm-dynamic-item-delete.md) | When a custom type CRM object is deleted ||
    || [onCrmDynamicItemUpdate](./events/on-crm-dynamic-item-update.md) | When a custom type CRM object is modified ||
    |#

{% endlist %}

### Custom Fields

CRM Object Identifier **entityId** — `CRM_SMART_INVOICE`

#|
|| **Method** | **Description** ||
|| [userfieldconfig.add](./userfieldconfig/userfieldconfig/userfieldconfig-add.md) | Creates a custom field ||
|| [userfieldconfig.update](./userfieldconfig/userfieldconfig/userfieldconfig-update.md) | Modifies field settings ||
|| [userfieldconfig.get](./userfieldconfig/userfieldconfig/userfieldconfig-get.md) | Returns the settings of a custom field by identifier ||
|| [userfieldconfig.getTypes](./userfieldconfig/userfieldconfig/userfieldconfig-get-types.md) | Returns the set of available types of custom fields for the module ||
|| [userfieldconfig.list](./userfieldconfig/userfieldconfig/userfieldconfig-list.md) | Returns a list of custom field settings ||
|| [userfieldconfig.delete](./userfieldconfig/userfieldconfig/userfieldconfig-delete.md) | Deletes a custom field ||
|#

### Product Items

CRM Object Identifier **ownerType** — `SI`

#|
|| **Method** | **Description** ||
|| [crm.item.productrow.add](./product-rows/crm-item-productrow-add.md) | Adds a product item ||
|| [crm.item.productrow.update](./product-rows/crm-item-productrow-update.md) | Updates a product item ||
|| [crm.item.productrow.get](./product-rows/crm-item-productrow-get.md) | Retrieves information about a product item by id ||
|| [crm.item.productrow.set](./product-rows/crm-item-productrow-set.md) | Associates a product item with a CRM object ||
|| [crm.item.productrow.list](./product-rows/crm-item-productrow-list.md) | Retrieves a list of product items ||
|| [crm.item.productrow.getAvailableForPayment](./product-rows/crm-item-productrow-get-available-for-payment.md) | Retrieves a list of unpaid products ||
|| [crm.item.productrow.delete](./product-rows/crm-item-productrow-update.md) | Deletes a product item ||
|| [crm.item.productrow.fields](./product-rows/crm-item-productrow-fields.md) | Retrieves a list of product item fields ||
|#

### Payments

CRM Object Identifier **entityTypeId** — `31`

#|
|| **Method** | **Description** ||
|| [crm.item.payment.add](./payment/crm-item-payment-add.md) | Creates a payment for a CRM object ||
|| [crm.item.payment.update](./payment/crm-item-payment-update.md) | Modifies the set of payment fields ||
|| [crm.item.payment.get](./payment/crm-item-payment-get.md) | Retrieves brief information about a payment ||
|| [crm.item.payment.list](./payment/crm-item-payment-list.md) | Retrieves a list of payments for a specific CRM object ||
|| [crm.item.payment.delete](./payment/crm-item-payment-delete.md) | Deletes a payment ||
|| [crm.item.payment.pay](./payment/crm-item-payment-pay.md) | Changes the payment status to "Paid" ||
|| [crm.item.payment.unpay](./payment/crm-item-payment-unpay.md) | Changes the payment status to "Unpaid" ||
|#

#### Product Items in Payment

#|
|| **Method** | **Description** ||
|| [crm.item.payment.product.add](./payment/products-in-payment/crm-item-payment-product-add.md) | Adds a product item to the payment ||
|| [crm.item.payment.product.list](./payment/products-in-payment/crm-item-payment-product-list.md) | Retrieves a list of product items in the payment ||
|| [crm.item.payment.product.delete](./payment/products-in-payment/crm-item-payment-product-delete.md) | Deletes a product item from the payment ||
|| [crm.item.payment.product.setQuantity](./payment/products-in-payment/crm-item-payment-product-set-quantity.md) | Changes the quantity of a product in the payment item ||
|#

#### Delivery in Payment

#|
|| **Method** | **Description** ||
|| [crm.item.payment.delivery.add](./payment/delivery-in-payment/crm-item-payment-delivery-add.md) | Adds a delivery item to the payment ||
|| [crm.item.payment.delivery.list](./payment/delivery-in-payment/crm-item-payment-delivery-list.md) | Retrieves a list of delivery items for a specific payment ||
|| [crm.item.payment.delivery.delete](./payment/delivery-in-payment/crm-item-payment-delivery-delete.md) | Deletes a delivery item from the payment ||
|| [crm.item.payment.delivery.setDelivery](./payment/delivery-in-payment/crm-item-payment-delivery-set-delivery.md) | Reassociates the delivery item with another delivery document ||
|#

### Managing Invoice Detail Form Settings

CRM Object Identifier **entityTypeId** — `31`

#|
|| [crm.item.details.configuration.forceCommonScopeForAll](./item-details-configuration/crm-item-details-configuration-forceCommonScopeForAll.md) | Sets a common detail form for all users ||
|| [crm.item.details.configuration.get](./item-details-configuration/crm-item-details-configuration-get.md) | Retrieves the parameters of the detail form for entities ||
|| [crm.item.details.configuration.reset](./item-details-configuration/crm-item-details-configuration-reset.md) | Resets the parameters of the detail form for entities ||
|| [crm.item.details.configuration.set](./item-details-configuration/crm-item-details-configuration-set.md) | Sets the parameters of the detail form for entities ||
|#