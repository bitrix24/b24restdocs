# Linking Objects to a Waitlist Entry: Overview of Methods

You can link additional objects to a waitlist entry. This helps synchronize sales and utilize automation, such as Automation rules in a deal.

> Quick navigation: [all methods](#all-methods)

## Connection to Objects

**Waitlist.** To create a new link for a waitlist entry, specify the `ID` of the entry in the `waitListId` parameter. You can obtain the `ID` using the [creation](../booking-v1-waitlist-add.md) or [filtering](../booking-v1-waitlist-list.md) methods.

{% note info "" %}

Currently, only CRM deals can be linked to a waitlist entry. Other modules and object types are not supported yet.

{% endnote %}

**Deal.** To create a link to a deal, pass the `ID` of the deal in the `value` parameter. You can obtain the `ID` using the [creation](../../../crm/deals/crm-deal-add.md) or [filtering](../../../crm/deals/crm-deal-list.md) methods. Use fixed values for the parameters `moduleId = crm` and `entityTypeId = DEAL`.

## Overview of Methods {#all-methods}

> Scope: [`booking`](../../../scopes/permissions)
>
> Who can perform the methods: any user

#|
|| **Method** | **Description** ||
|| [booking.v1.waitlist.externalData.list](./booking-v1-waitlist-externaldata-list.md) | Retrieves links for a waitlist entry ||
|| [booking.v1.waitlist.externalData.set](./booking-v1-waitlist-externaldata-set.md) | Establishes links for a waitlist entry ||
|| [booking.v1.waitlist.externalData.unset](./booking-v1-waitlist-externaldata-unset.md) | Removes links for a waitlist entry ||
|#