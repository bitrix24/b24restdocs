# Recurring Deals: Overview of Methods

Recurring deals are deals that are created based on a template. You can set the period and the number of repetitions for recurring deals. They will be automatically created in the selected Sales Funnel.

> Quick navigation: [all methods and events](#all-methods) 
>
> User documentation: [Recurring deals](https://helpdesk.bitrix24.com/open/17240254/)

## Connection of Recurring Deals with Other CRM Objects

**Deals.** To create a new template, use the method [crm.deal.recurring.add](./crm-deal-recurring-add.md). Pass the ID of an existing deal in the `DEAL_ID` parameter of the method. The field values of this deal will be saved in the template.

To view the deal based on which the template was created, execute the method [crm.deal.recurring.list](./crm-deal-recurring-list.md). As a result, you will receive the value `BASED_ID`. Pass the obtained value to the `id` parameter of the method [crm.deal.get](../crm-deal-get.md). If the template was created in the recurring deals section of Bitrix24, the `BASED_ID` parameter will be empty.

**Funnels.** You can create and manage sales funnels for deals through the group of methods [crm.category.*](../../universal/category/index.md) where `entityTypeId` of the deal = `2`. To create a recurring deal in the desired funnel, pass the funnel ID in the `CATEGORY_ID` parameter.

**Products.** If the deal template includes product items, a new recurring deal will be created with them. To modify the product items in the template, use the methods from the group [crm.item.productrow.*](../../universal/product-rows/index.md). Pass the `DEAL_ID` value from the result of the method [crm.deal.recurring.list](./crm-deal-recurring-list.md) as the `ownerId` of the deal.

**Clients.** If a company and contacts are selected in the deal template, a new recurring deal will be created with them. To change the company in the template, use the method [crm.deal.update](../crm-deal-update.md), and to change the contacts — the methods from the group [crm.deal.contact.*](../contacts/crm-deal-contact-add.md). Pass the `DEAL_ID` value from the result of the method [crm.deal.recurring.list](./crm-deal-recurring-list.md) as the `id` of the deal.

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
> 
> Who can execute the method: depending on the method

{% list tabs %}

- Methods
  
    #|
    || **Method** | **Description** ||
    || [crm.deal.recurring.add](./crm-deal-recurring-add.md) | Creates a new recurring deal template ||
    || [crm.deal.recurring.fields](./crm-deal-recurring-fields.md) | Returns a list of fields for the recurring deal template ||
    || [crm.deal.recurring.expose](./crm-deal-recurring-expose.md) | Creates a new deal based on the template ||
    || [crm.deal.recurring.update](./crm-deal-recurring-update.md) | Modifies the settings of the recurring deal template ||
    || [crm.deal.recurring.get](./crm-deal-recurring-get.md) | Returns the settings of the recurring deal template by Id ||
    || [crm.deal.recurring.list](./crm-deal-recurring-list.md) | Returns a list of recurring deal templates ||
    || [crm.deal.recurring.delete](./crm-deal-recurring-delete.md) | Deletes a recurring deal template ||
    |#

- Events
  
    #|
    || **Event** | **Triggered** ||
    || [onCrmDealRecurringAdd](./events/on-crm-deal-recurring-add.md) | When a new recurring deal is created ||
    || [onCrmDealRecurringUpdate](./events/on-crm-deal-recurring-update.md) | When a recurring deal is modified ||
    || [onCrmDealRecurringDelete](./events/on-crm-deal-recurring-delete.md) | When a recurring deal is deleted ||
    || [onCrmDealRecurringExpose](./events/on-crm-deal-recurring-expose.md) | When a new deal is created from a recurring deal ||
    |#

{% endlist %}