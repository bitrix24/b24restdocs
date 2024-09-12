# Invoices

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments to standards
- add links:
- - to constants, if available
- - to crm.enum.ownertype and crm.status.entity.types after Auxiliary Entities
- - to SPAs
- - to methodscrmcategory/category.php  

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

**New invoices** are a distinct type of entity that replaces the previously existing [invoices](../outdated/index.md).

## As an SPA

New invoices are a fixed type of SPA, except for the details. Therefore, the same methods used for working with SPAs are also applicable to new invoices. The only difference lies in the values of the input parameters.

Many methods require specifying `entityTypeId`. For this parameter, you need to pass the `entityTypeId` for new invoices. The value of `entityTypeId` for new invoices, as well as other entity type-specific constants, can be found in the list of [constants](.).

Additionally, the values of `entityTypeId`, `entityTypeName`, and `ownerType` can be obtained through the method [crm.enum.ownertype](.) (see fields `ID`, `SYMBOL_CODE`, `SYMBOL_CODE_SHORT`).

## crm.type.*

It is **not possible** to read/change/delete new invoices through the methods [crm.type.*](.). An error will be returned if an attempt is made to do so.

## Managing new invoice items – crm.item.*

Through the methods [crm.item.*](.) you can [read](.)/[modify](.)/[delete](.)/[add](.) items of new invoices. Working with new invoices through these methods is no different from what is described in the documentation. For the `entityTypeId` parameter, you need to pass the value relevant for new invoices.

## Managing product items – crm.item.productrow.*

Through the methods [crm.item.productrow.*](../productrow/index.md) you can manage product items associated with the invoice. Working with them is similar to that for SPA items. For the `ownerType` parameter, you need to pass the short symbolic code for new invoices (`SI`).

## Managing stages – crm.status.*

Through the methods [crm.status.*](.) you can manage the stages of new invoices. The value of the `ENTITY_ID` and `STATUS_ID` fields is formed in the same way as for SPAs.

To obtain the ready `ENTITY_ID` value for new invoices, you can use the method [crm.status.entity.types](.) and find the subarray where `ENTITY_TYPE_ID` matches the `entityTypeId` for new invoices. The required value will be under the key `ID`, which can be used as `ENTITY_ID` in other methods.

## crm.category.*

Since new invoices are based on SPAs, they technically have the capability to work with categories. However, it is **not recommended** to use the methods [crm.category.*](.) to change the categories of new invoices, as categories are not utilized in them. Any changes will not be reflected in the interface.

## Invoice detail form settings – crm.item.details.configuration.*

Through the methods [crm.item.details.configuration.*](.) you can manage the settings of the new invoice detail form. For `entityTypeId`, you need to pass the value relevant for the new invoice.

## Events on new invoice items

When adding/modifying/deleting an item of a new invoice, events identical to those of the [SPA](.) will be triggered. The identifier for the new invoice CRM (`entityTypeId`) will be passed as the value for `ENTITY_TYPE_ID`.

## Widget codes in the interface

Widget codes are generated in the same way as for other entities, but the string name used is `SMART_INVOICE`, for example:
- `CRM_SMART_INVOICE_LIST_TOOLBAR` – button in the toolbar in the invoice list
- `CRM_SMART_INVOICE_DETAIL_TOOLBAR` – button in the toolbar in the invoice detail form
- `CRM_SMART_INVOICE_DOCUMENTGENERATOR_BUTTON` – menu item in the "Document" button in the invoice detail form

... and others