# About Deals

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

From Sergey’s file:
What is this object, what standard types exist, how they appear in the interface. We mention that there are some types of email objects that are processed using old methods, here’s the link. There should be a well-thought-out landing page that explains when to use crm.activity.add, when to use crm.activity.todo.add, and when to use a configurable deal.

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

Deals describe completed, current, and planned work related to leads, contacts, companies, and deals. They are divided into calls, meetings, and e-mails.

Three types of deals:
- System
- Universal
- Configurable

## Configurable Deal

Methods for working with a configurable deal:

#|
|| **Method** | **Description** | **Version** ||
|| [crm.activity.configurable.get](./crm-activity-configurable-get.md) | Information about the deal. | 23.0.0 ||
|| [crm.activity.configurable.add](./crm-activity-configurable-add.md) | Add a deal | 23.0.0 ||
|| [crm.activity.configurable.update](./crm-activity-configurable-update.md) | Update a deal | 23.0.0 ||
|#

General methods for the Deal entity that can be used:

1. To delete a deal, you can use the general method for deals [crm.activity.delete](.).
2. To get a list of deals, you can use the general method for deals [crm.activity.list](.) with a filter for `PROVIDER_ID = CONFIGURABLE_REST_APP`.