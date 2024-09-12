# Product Items

## Description

REST methods from the **crm.item.productrow.\*** family allow you to work with product items linked to various CRM entities. These methods are universal and support any type of owner that can interact with the [new CRM API](https://training.bitrix24.com/api_d7/bitrix/crm/crm_new_api.php).

## Access Permissions

When calling REST methods, the access permissions of the user making the call are taken into account. Product items are not standalone CRM entities and are always associated with some CRM object that acts as their owner. Therefore, for all operations, the user's permissions to access and manage the owner object (estimate, SPA, etc.) are checked. For example, if a user does not have read access to a particular entity, they will also be unable to read its product items.

## Automatic Actions After Any Change

After any changes made to product items, all standard checks and procedures that occur when modifying a CRM object will be executed, including recalculating the total amount and triggering automation rules after saving. This applies to all methods that create or modify product items: [crm.item.productrow.add](./crm-item-productrow-add.md), [crm.item.productrow.update](./crm-item-productrow-update.md), [crm.item.productrow.set](./crm-item-productrow-set.md).

## Overview of Methods

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

#|
|| **Method** | **Description** ||
|| [crm.item.productrow.add](./crm-item-productrow-add.md) | Adds a product item ||
|| [crm.item.productrow.update](./crm-item-productrow-update.md) | Updates a product item ||
|| [crm.item.productrow.get](./crm-item-productrow-get.md) | Retrieves information about a product item by id ||
|| [crm.item.productrow.set](./crm-item-productrow-set.md) | Associates a product item with a CRM object ||
|| [crm.item.productrow.list](./crm-item-productrow-list.md) | Retrieves a list of product items ||
|| [crm.item.productrow.getAvailableForPayment](./crm-item-productrow-get-available-for-payment.md) | Retrieves a list of unpaid products ||
|| [crm.item.productrow.delete](./crm-item-productrow-update.md) | Deletes a product item ||
|| [crm.item.productrow.fields](./crm-item-productrow-fields.md) | Retrieves a list of product item fields ||
|#