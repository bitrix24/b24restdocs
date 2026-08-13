# Call List: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A call list is a CRM activity from which multiple clients can be called consecutively. An employee works with a single activity that contains selected contacts or companies. The results of the calls will be automatically saved in the client cards:

- in the attached call activity and its comments
- in the filled CRM form and fields of the card
- in the deal or invoice created from the call list

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Calling in Bitrix24](https://helpdesk.bitrix24.com/open/21815426/)

## Getting Started

1. Retrieve the identifiers of the call list participants — contacts or companies — using the [crm.item.list](../universal/crm-item-list.md) method

2. Create a call list using the [crm.calllist.add](./crm-calllist-add.md) method: pass the object type in the `ENTITY_TYPE` parameter and the array of identifiers in the `ENTITIES` parameter. If necessary, attach a CRM form using the `WEBFORM_ID` parameter

3. Check the composition of the call list using the [crm.calllist.items.get](./crm-calllist-items-get.md) method, and the parameters of the call list itself using the [crm.calllist.get](./crm-calllist-get.md) method

4. The employee then calls the participants in the Bitrix24 interface. The statuses of the participants during the call list are returned by the same [crm.calllist.items.get](./crm-calllist-items-get.md) method, and the list of possible statuses is returned by the [crm.calllist.statuslist](./crm-calllist-statuslist.md) method

5. To add or remove participants of an already created call list, use the [crm.calllist.update](./crm-calllist-update.md) method

## Relationships with Other Objects

**Contact**. To call multiple contacts, specify an array of `ID` contacts in the [creation](./crm-calllist-add.md) or [update](./crm-calllist-update.md) method of the call list. You can obtain the `ID` of contacts using the [crm.item.list](../universal/crm-item-list.md) method with the parameter `entityTypeId = 3`.

**Company**. To call multiple companies, specify an array of `ID` companies in the [creation](./crm-calllist-add.md) or [update](./crm-calllist-update.md) method of the call list. You can obtain the `ID` of companies using the [crm.item.list](../universal/crm-item-list.md) method with the parameter `entityTypeId = 4`.

**CRM Form**. During the call, the employee can fill in information about the client in the CRM form attached to the call list. After the call is completed, the form will be available in the client card, and the information from the form will be saved in the fields of the card. To attach a form to the call list, specify its `ID` in the [creation](./crm-calllist-add.md) or [update](./crm-calllist-update.md) method of the call list. The `ID` of the form can be found in the list of forms in Bitrix24 at `https://your-domain.com/crm/webform/`.

**Directory**. The status of the call in the call list card is an element of the [directory](../status/index.md). The list of statuses available to the call list participants is returned by the [crm.calllist.statuslist](./crm-calllist-statuslist.md) method. To change the statuses themselves, use the directory methods [crm.status.*](../status/index.md) with the parameter `ENTITY_ID = CALL_LIST`.

**User**. The call list is linked to its creator by a numeric identifier in the `CREATED_BY_ID` field. This field is returned by the [crm.calllist.get](./crm-calllist-get.md) and [crm.calllist.list](./crm-calllist-list.md) methods. You can obtain user data by identifier using the [user.get](../../user/user-get.md) method.

## How to Delete a Call List

There is no method for deleting a call list among the call list methods. You can delete a call list by deleting the activity using the [crm.activity.delete](../timeline/activities/activity-base/crm-activity-delete.md) method.

1. Use the [crm.activity.list](../timeline/activities/activity-base/crm-activity-list.md) method with a filter by the activity name `SUBJECT`. In the field value, specify `Call list #ID`. Replace `ID` with the identifier of the call list obtained from the [crm.calllist.add](./crm-calllist-add.md) or [crm.calllist.list](./crm-calllist-list.md) methods.

2. Pass the `ID` of the activity from the result to the deletion method [crm.activity.delete](../timeline/activities/activity-base/crm-activity-delete.md).

## Access Permissions

Permissions depend on the operation:

- a user with read access to the specified contacts or companies can create and update a call list, as well as retrieve its participants: objects unavailable to the user will not be included in the call list, and the [crm.calllist.items.get](./crm-calllist-items-get.md) method will return only the available ones
- any user can retrieve the parameters of a call list and the list of call lists

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [crm.calllist.add](./crm-calllist-add.md) | Creates a new call list ||
|| [crm.calllist.update](./crm-calllist-update.md) | Updates the composition of the call list ||
|| [crm.calllist.get](./crm-calllist-get.md) | Returns information about the call list ||
|| [crm.calllist.list](./crm-calllist-list.md) | Returns a list of all call lists ||
|| [crm.calllist.items.get](./crm-calllist-items-get.md) | Returns participants of the call list ||
|| [crm.calllist.statuslist](./crm-calllist-statuslist.md) | Returns a list of call list participant statuses ||
|#
