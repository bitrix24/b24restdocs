# Editing Data in CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Editing data in CRM means changing values that are already retained in cards and related objects: fields of leads, contacts, companies, and deals, phone numbers and emails, activity bindings, and the payment date in a deal field.

A scenario is a sequence of requests for a single task. It describes the order of method calls, provides a code example, and states the result.

The tables below help you select a scenario by task, main methods, and result. Scenarios for creating CRM objects are collected in [How to Add Data to CRM](../how-to-add-crm-objects/index.md), and selections and lists in [Retrieving Lists in CRM](../how-to-get-lists/index.md).

## What You Need

**Scope.** All scenarios require the [`crm`](../../../api-reference/scopes/permissions.md) scope. Scenarios with edit forms additionally require access to user data — the [user.get](../../../api-reference/user/user-get.md) method works with the [`user_brief`](../../../api-reference/scopes/permissions.md), `user_basic`, or `user` scope.

**Permissions.** The user needs permission to edit the CRM item the scenario works with. Edit forms additionally require access to CRM settings.

The exact permissions and scope of a specific scenario are listed in the header of its page.

## Getting Started

1. Select a scenario from the table of the relevant group.
2. Create an [incoming webhook](../../../local-integrations/local-webhooks.md#incoming-webhook) or an application with the required scopes, and check the user permissions.
3. Retrieve the identifiers the scenario starts with: a CRM item, an activity, a custom field, or a payment.
4. Execute the methods in the order described in the scenario.

## Editing CRM Object Cards

An edit form is built from the field description rather than from a predefined list. The generator retrieves the fields using the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method and selects an HTML form element for the type of each field, so custom fields appear in the form without changing the code.

**Object Type.** The universal `crm.item.*` methods identify a CRM item by a pair of values: `entityTypeId` for the object type and `id` for the item itself. The object type values are `1` for a lead, `2` for a deal, `3` for a contact, and `4` for a company. The complete list is available in the [CRM object types](../../../api-reference/crm/data-types.md#object_type) table.

**Reference Guides.** Service codes in fields are replaced with readable names by additional methods: [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) returns stages and other list fields, [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) returns pipelines, [crm.currency.list](../../../api-reference/crm/currency/crm-currency-list.md) returns currencies, and [user.get](../../../api-reference/user/user-get.md) returns employees.

**Method Branch.** The card scenarios are built on the [universal methods](../../../api-reference/crm/universal/index.md) `crm.item.*`: they work with all CRM objects, including smart processes. Development of the `crm.lead.*`, `crm.contact.*`, `crm.company.*`, and `crm.deal.*` methods has been discontinued — they continue to work, and some of the scenarios below call them, but for new development choose `crm.item.*`. Custom field methods, such as `crm.deal.userfield.*`, continue to work as before.

#|
|| **Scenario** | **Main Methods** | **Result** ||
|| [How to Create a Custom Lead Edit Form](./how-to-generate-edit-form-for-lead.md) | [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md), [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md), [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md), [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) with `entityTypeId = 1` | A web form that creates a lead or updates an existing one ||
|| [How to Create a Custom Contact Edit Form](./how-to-make-contact-edit-card.md) | [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md), [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md), [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md), [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) with `entityTypeId = 3` | A web form that creates a contact or updates an existing one ||
|| [How to Create Your Own Company Edit Card](./how-to-generate-edit-form-for-company.md) | [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md), [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md), [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md), [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) with `entityTypeId = 4` | A web form that creates a company or updates an existing one ||
|| [How to Create a Custom Deal Edit Form](./how-to-generate-edit-form-for-deal.md) | [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md), [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md), [crm.status.list](../../../api-reference/crm/status/crm-status-list.md), [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md), [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) with `entityTypeId = 2` | A web form with pipeline and stage selection that creates a deal or updates an existing one ||
|#

## Changing Phone Numbers and Emails

Phone numbers and email addresses are retained in the `fm` multifield — a set of entries of the [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) type. Every entry has its own `id`, which Bitrix24 assigns when the entry is created.

An entry cannot be found by the text of its value, so the object is read first to retrieve the identifiers, and only then the changes are sent. In the update method, the operation is determined by the key of an element in the `fm` object: a numeric `id` changes the entry, the same `id` with an empty `value` deletes it, and the keys `n0`, `n1` add new ones.

#|
|| **Scenario** | **Main Methods** | **Result** ||
|| [How to Change or Delete Phone Numbers and Emails](./how-to-change-email-or-phone.md) | [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md), [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md), [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) | An updated or cleared list of phone numbers and emails in the contact card ||
|#

## Moving Activities and Changing Deadlines

An activity is a record in the card timeline: a call, a meeting, an email, or a planned action. Activities are linked to CRM items through bindings. An activity can have several bindings, but the last one cannot be deleted — the method returns the `LAST_BINDING_CANNOT_BE_DELETED` error.

**Moving Within One Type.** If the source and target objects are of the same type, the binding is moved by a single method, [crm.activity.binding.move](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-move.md).

**Moving Between Different Types.** The `crm.activity.binding.move` method is not suitable here: it returns the `SOURCE_AND_TARGET_ENTITY_TYPES_ARE_NOT_EQUAL_ERROR` error. Such a move is assembled from two operations — first the new binding is added, then the old one is deleted.

**Deadlines.** The deadline and reminders are changed by the [crm.activity.todo.update](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-update.md) method. It does not update a completed activity and returns the `CAN_NOT_UPDATE_COMPLETED_TODO` error, so the activity is first found with the `COMPLETED: 'N'` filter.

#|
|| **Scenario** | **Main Methods** | **Result** ||
|| [How to Reschedule a Planned Activity](./how-to-change-date-in-activity.md) | [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md), [crm.activity.todo.update](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-update.md) | A new deadline and reminders for a planned activity ||
|| [How to Move an Activity Between Items of the Same Type](./how-to-move-activity.md) | [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md), [crm.activity.binding.move](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-move.md) | The activity in the timeline of another item of the same type ||
|| [How to Move an Activity from One Object Type to Another](./how-to-move-activity-between-objects.md) | [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md), [crm.activity.binding.add](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-add.md), [crm.activity.binding.delete](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-delete.md) | The activity in the timeline of an object of another type, for example from a lead to a company ||
|#

## Retaining the Payment Date in a Deal Field

The payment date is stored in the payment document, not in the deal. It is transferred to a custom field of the deal when the date is needed by an external system, a BI Builder report, an automation rule, or a workflow.

The custom field is created in advance in the CRM settings. Its identifier is different in every Bitrix24, so the field is located by its name: the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method returns the set of deal fields, where the key is an identifier of the `ufCrm_*` form and `title` is the field name in the card.

#|
|| **Scenario** | **Main Methods** | **Result** ||
|| [How to Save the Payment Date in the Deal Field](./how-to-set-paid-date-to-deal.md) | [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md), [crm.item.payment.list](../../../api-reference/crm/universal/payment/crm-item-payment-list.md), [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) | The payment date in a custom field of the deal ||
|#

## Continue Learning

- [{#T}](../how-to-add-crm-objects/index.md)
- [{#T}](../how-to-get-lists/index.md)
- [{#T}](../../../api-reference/crm/universal/index.md)
- [{#T}](../../../api-reference/crm/timeline/activities/binding/index.md)
- [{#T}](../../../api-reference/crm/data-types.md)
