# Recurring Deals: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A recurring deal is created automatically from a template with a defined period and number of repetitions. The template retains the field values of future deals, and the repetition settings define how often and until when new deals are created in the selected Sales Funnel.

{% note warning "" %}

Recurring deals are not available on every Bitrix24 plan. If the tool is unavailable, the methods [crm.deal.recurring.get](./crm-deal-recurring-get.md), [crm.deal.recurring.update](./crm-deal-recurring-update.md), and [crm.deal.recurring.delete](./crm-deal-recurring-delete.md) return the error `Recurring is not allowed`.

{% endnote %}

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Recurring deals](https://helpdesk.bitrix24.com/open/17240254/)

## Identifiers of a Recurring Deal Template

A template consists of two objects: the template deal, which retains the field values, and the record with the repetition settings. They have different identifiers and are not interchangeable in the methods.

#|
|| **Identifier** | **What It Denotes** | **Where It Is Used** ||
|| `ID` | Identifier of the repetition settings | The `id` parameter of the methods [crm.deal.recurring.get](./crm-deal-recurring-get.md), [crm.deal.recurring.update](./crm-deal-recurring-update.md), [crm.deal.recurring.delete](./crm-deal-recurring-delete.md), and [crm.deal.recurring.expose](./crm-deal-recurring-expose.md) ||
|| `DEAL_ID` | Identifier of the template deal from which the field values are copied | The `id` parameter of the deal methods — [crm.deal.get](../crm-deal-get.md), [crm.deal.update](../crm-deal-update.md) ||
|| `BASED_ID` | Identifier of the source deal the template was made from | The `id` parameter of the method [crm.deal.get](../crm-deal-get.md) ||
|#

The method [crm.deal.recurring.add](./crm-deal-recurring-add.md) handles a regular deal and a template deal differently.

- If you pass a regular deal in `DEAL_ID`, Bitrix24 creates a copy of it, and that copy becomes the template deal. In the repetition settings, `DEAL_ID` points to this copy, and `BASED_ID` points to the source deal. Product items are copied into the template along with the fields
- If you pass a deal that is already marked as a template in `DEAL_ID`, the repetition settings are linked to it directly, and `BASED_ID` remains empty. Settings cannot be added to the same deal twice — the method returns the error `Deal already have had recurring settings`

Both identifiers can be retrieved using the method [crm.deal.recurring.list](./crm-deal-recurring-list.md).

## Getting Started

1. Create a deal using the method [crm.deal.add](../crm-deal-add.md) or take an existing one — its fields become the basis of the template
2. Find out the identifier of the funnel in which the deals are to be created using the method [crm.category.list](../../universal/category/crm-category-list.md) with the parameter `entityTypeId = 2`
3. Create the template using the method [crm.deal.recurring.add](./crm-deal-recurring-add.md): pass `DEAL_ID`, the funnel in `CATEGORY_ID`, the date of the first run in `START_DATE`, and the frequency in `PARAMS`
4. Limit the number of repetitions with the fields `IS_LIMIT`, `LIMIT_REPEAT`, and `LIMIT_DATE` if the deals are not meant to be created indefinitely
5. Check the result using the method [crm.deal.recurring.get](./crm-deal-recurring-get.md) — it returns the date of the next run `NEXT_EXECUTION` and the counter of created deals `COUNTER_REPEAT`
6. Create a deal from the template outside the schedule using the method [crm.deal.recurring.expose](./crm-deal-recurring-expose.md)
7. Subscribe to the [recurring deal events](./events/index.md) to receive notifications in your application

## Connection of Recurring Deals with Other CRM Objects

**Deals.** The field values of future deals are retained by the template deal. To view or change them, take `DEAL_ID` from the result of the method [crm.deal.recurring.list](./crm-deal-recurring-list.md) and pass it to the `id` parameter of the methods [crm.deal.get](../crm-deal-get.md) and [crm.deal.update](../crm-deal-update.md). Every deal created from a template is a regular deal, so it triggers the event [onCrmDealAdd](../events/on-crm-deal-add.md) along with the event [onCrmDealRecurringExpose](./events/on-crm-deal-recurring-expose.md).

**Funnels.** Sales funnels are managed by the group of methods [crm.category.*](../../universal/category/index.md) with `entityTypeId = 2`. To have deals created from the template in the required funnel, pass its identifier in the `CATEGORY_ID` field. You can retrieve the list of funnels using the method [crm.category.list](../../universal/category/crm-category-list.md).

**Products.** The product items of the template deal are copied into every new deal. You can change them with the group of methods [crm.item.productrow.*](../../universal/product-rows/index.md): pass `ownerType = D` and the `DEAL_ID` value from the result of the method [crm.deal.recurring.list](./crm-deal-recurring-list.md) in the `ownerId` parameter.

**Clients.** The company and contacts of the template deal are also carried over to new deals. The company is changed by the method [crm.deal.update](../crm-deal-update.md) through the `COMPANY_ID` field, and the contacts by the group of methods [crm.deal.contact.*](../contacts/index.md). Take the identifier of the template deal from the `DEAL_ID` field of the method [crm.deal.recurring.list](./crm-deal-recurring-list.md).

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method — all methods check the access permissions for deals. Reading the settings requires the "read" access permission for deals, creating a template requires the "add" and "modify" permissions, and deleting one requires the "delete" permission

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [crm.deal.recurring.add](./crm-deal-recurring-add.md) | Creates a recurring deal template ||
    || [crm.deal.recurring.update](./crm-deal-recurring-update.md) | Modifies the settings of the recurring deal template ||
    || [crm.deal.recurring.get](./crm-deal-recurring-get.md) | Returns the settings of the recurring deal template by its identifier ||
    || [crm.deal.recurring.list](./crm-deal-recurring-list.md) | Returns a list of recurring deal templates ||
    || [crm.deal.recurring.delete](./crm-deal-recurring-delete.md) | Deletes a recurring deal template ||
    || [crm.deal.recurring.expose](./crm-deal-recurring-expose.md) | Creates a deal from the template outside the schedule ||
    || [crm.deal.recurring.fields](./crm-deal-recurring-fields.md) | Returns the description of the recurring deal template fields ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [onCrmDealRecurringAdd](./events/on-crm-deal-recurring-add.md) | When a recurring deal template is created ||
    || [onCrmDealRecurringUpdate](./events/on-crm-deal-recurring-update.md) | When a recurring deal template is modified ||
    || [onCrmDealRecurringDelete](./events/on-crm-deal-recurring-delete.md) | When a recurring deal template is deleted ||
    || [onCrmDealRecurringExpose](./events/on-crm-deal-recurring-expose.md) | When a deal is created from the template ||
    |#

{% endlist %}
