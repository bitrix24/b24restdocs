# Activities in REST 3.0: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Activities are calls, meetings, emails, tasks, and other actions employees perform with a client. An activity is bound to one or more CRM items and is shown in their cards. REST 3.0 methods read activities, complete and reopen them, delete them, change their bindings to items, and work with CRM emails: read the text and the thread, and send a reply.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Activities in CRM](https://helpdesk.bitrix24.com/open/21648890/)

{% note warning "" %}

REST 3.0 methods cannot yet create or update activities. Use the previous version methods: [crm.activity.todo.add](../../timeline/activities/todo/crm-activity-todo-add.md) for a universal activity and [crm.activity.add](../../timeline/activities/activity-base/crm-activity-add.md) for activities of other types.

{% endnote %}

## How to Choose a Method

Methods for reading and changing an activity come in two variants: in the context of an object type and without it.

#|
|| **Variant** | **Example** | **When to Use** ||
|| In the context of an object type | `crm.deal.activity.get`, `crm.smartprocess1038.activity.list` | Working with the activities of a single CRM item. The method finds only activities bound to an item of this type. The list of item activities is returned by [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) ||
|| Without an object type | `crm.activity.get`, `crm.activity.list` | The activity is known by ID, and what it is bound to does not matter. [crm.activity.list](./crm-activity-list-rest-v3.md) returns activities of the entire Bitrix24 and is available only to an administrator ||
|#

The object type is set by the `<type>` segment of the method name:

#|
|| **Object type** | **Segment in the method name** ||
|| Deal | `deal` ||
|| Lead | `lead` ||
|| Contact | `contact` ||
|| Company | `company` ||
|| Estimate | `quote` ||
|| Invoice | `smartinvoice` ||
|| CRM document | `smartdocument` ||
|| SPA | `smartprocess<entityTypeId>`, for example, `smartprocess1038` ||
|#

My company and B2E documents have no activity methods. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field. Method names are written in lowercase: `crm.deal.activity.get`. The exception is the email methods `crm.activity.mail.getContent` and `crm.activity.mail.getThread`: their names contain capital letters, as in the method table.

## Relationship with Other Objects

An activity is linked to CRM items, employees, and mail.

**CRM items.** An activity is bound to one or more items: the bindings are returned in the `bindings` field as `entityTypeId` and `entityId` pairs. Permission for an activity follows from the permissions for the items it is bound to. Bindings are changed by the [crm.activity.binding.bind](./crm-activity-binding-bind-rest-v3.md), [unbind](./crm-activity-binding-unbind-rest-v3.md), and [move](./crm-activity-binding-move-rest-v3.md) methods. The last binding cannot be removed; it can only be moved to another item. Object type IDs are returned by the [crm.dictionary.entitytype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method.

**Employees.** `assignedById` contains the responsible person, `authorId` the author, and `editorId` the last person who changed the activity. Employee data can be retrieved with the [user.get](../../../user/user-get.md) method.

**Mail.** CRM emails are activities with `providerId` equal to `CRM_EMAIL`. The email text is returned by [crm.activity.mail.getContent](./crm-activity-mail-get-content-rest-v3.md), the entire conversation by [crm.activity.mail.getThread](./crm-activity-mail-get-thread-rest-v3.md), and a reply is sent by [crm.activity.mail.reply](./crm-activity-mail-reply-rest-v3.md).

## Activity Fields {#fields}

The `get` and `list` methods without `select` return all fields from the table. To retrieve some of the fields, list them in `select`. The fields are read-only: REST 3.0 methods do not change activity fields.

#|
|| **Field**
`type` | **Description** | **Filter** | **Sorting** ||
|| **id**
[`integer`](../../../data-types.md) | Activity ID | Yes | Yes ||
|| **type**
[`string`](../../../data-types.md) | Kind of activity.

Possible values:

- `meeting` — meeting
- `call` — call
- `task` — task
- `email` — email
- `activity` — activity
- `provider` — an activity managed by a provider, for example, the universal activity `CRM_TODO` | Yes | No ||
|| **providerId**
[`string`](../../../data-types.md) | Activity provider, for example, `CRM_TODO`, `CRM_EMAIL`, `VOXIMPLANT_CALL`. Providers are returned by the [crm.dictionary.activityprovider.list](../dictionaries/crm-dictionary-list-rest-v3.md) method | Yes | No ||
|| **providerTypeId**
[`string`](../../../data-types.md) | Activity subtype within the provider, for example, `TODO` | Yes | No ||
|| **subject**
[`string`](../../../data-types.md) | Activity subject | No | No ||
|| **description**
[`string`](../../../data-types.md) | Activity description. The format is set by `descriptionType` | No | No ||
|| **descriptionType**
[`string`](../../../data-types.md) | Description format: `plain` — text, `bbcode` — BB code, `html` — HTML | No | No ||
|| **isCompleted**
[`boolean`](../../../data-types.md) | `true` — the activity is completed, `false` — the activity is open | Yes | No ||
|| **assignedById**
[`integer`](../../../data-types.md) | ID of the responsible person | Yes | No ||
|| **authorId**
[`integer`](../../../data-types.md) | Author ID | No | No ||
|| **editorId**
[`integer`](../../../data-types.md) | ID of the employee who last changed the activity | No | No ||
|| **originatorId**
[`string`](../../../data-types.md) | External system the activity was imported from. `null` if the activity was created in Bitrix24 | No | No ||
|| **originId**
[`string`](../../../data-types.md) | Activity ID in the external system | No | No ||
|| **isImportant**
[`boolean`](../../../data-types.md) | `true` — the activity has high importance | No | No ||
|| **direction**
[`string`](../../../data-types.md) | Communication direction: `incoming` — incoming, `outgoing` — outgoing, `undefined` — no direction | Yes | No ||
|| **isIncomingChannel**
[`boolean`](../../../data-types.md) | `true` — the activity came from an incoming request channel | No | No ||
|| **createdTime**
[`datetime`](../../../data-types.md) | Creation date and time | Yes | Yes ||
|| **updatedTime**
[`datetime`](../../../data-types.md) | Date and time of the last change | Yes | Yes ||
|| **startTime**
[`datetime`](../../../data-types.md) | Activity start | Yes | Yes ||
|| **endTime**
[`datetime`](../../../data-types.md) | Activity end | No | Yes ||
|| **deadlineTime**
[`datetime`](../../../data-types.md) | Deadline. `null` if there is no deadline | Yes | Yes ||
|| **bindings**
[`object[]`](../../../data-types.md) | CRM items the activity is bound to: `entityTypeId` is the object type, `entityId` is the item ID | No | No ||
|| **communications**
[`object[]`](../../../data-types.md) | Communication contacts: `type` is the channel, for example, `EMAIL` or `PHONE`, `value` is the address or number, `entityTypeId` and `entityId` are the CRM item the address belongs to. If you have no access to the item, `value`, `entityTypeId`, and `entityId` are `null` | No | No ||
|| **files**
[`object[]`](../../../data-types.md) | Activity files: `id`, `name`, `url`, `downloadUrl` | No | No ||
|#

Empty `bindings`, `communications`, and `files` are returned as an empty array `[]`. `select` accepts the `assignedBy`, `author`, and `editor` fields with employee data, but the response contains only the ID: `assignedById`, `authorId`, or `editorId`.

To find activities without a deadline, use the `["deadlineTime","=",null]` condition. When sorting by `deadlineTime`, activities without a deadline may appear both at the beginning and at the end of the list.

The descriptions of fields with the `filterable` and `sortable` attributes are returned by the `crm.<type>.activity.field.list` and `crm.<type>.activity.field.get` methods, and for methods without a type, by `crm.activity.field.list` and `crm.activity.field.get`. They work like the other REST 3.0 field methods: `field.list` accepts `select` with a list of attributes, and `field.get` also accepts `name` with the field name, for example, `{"name":"deadlineTime"}`. The binding field descriptions are returned by `crm.activity.binding.field.list` and `crm.activity.binding.field.get`.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### CRM Item Activities

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.activity.get](./crm-entity-activity-get-rest-v3.md) | Returns an activity bound to an item of this object type ||
|| [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) | Returns the activities of a single item ||
|| [crm.&lt;type&gt;.activity.delete](./crm-entity-activity-delete-rest-v3.md) | Deletes an activity ||
|| [crm.&lt;type&gt;.activity.complete](./crm-entity-activity-complete-rest-v3.md) | Completes an activity ||
|| [crm.&lt;type&gt;.activity.uncomplete](./crm-entity-activity-uncomplete-rest-v3.md) | Reopens an activity ||
|| `crm.<type>.activity.field.list` | Returns the description of activity fields ||
|| `crm.<type>.activity.field.get` | Returns the description of an activity field by name ||
|#

### Activities Without an Object Type

#|
|| **Method** | **Description** ||
|| [crm.activity.get](./crm-activity-get-rest-v3.md) | Returns an activity by ID ||
|| [crm.activity.list](./crm-activity-list-rest-v3.md) | Returns activities of the entire Bitrix24, for administrators only ||
|| [crm.activity.delete](./crm-activity-delete-rest-v3.md) | Deletes an activity ||
|| [crm.activity.complete](./crm-activity-complete-rest-v3.md) | Completes an activity ||
|| [crm.activity.uncomplete](./crm-activity-uncomplete-rest-v3.md) | Reopens an activity ||
|| `crm.activity.field.list` | Returns the description of activity fields ||
|| `crm.activity.field.get` | Returns the description of an activity field by name ||
|#

### Activity Bindings

#|
|| **Method** | **Description** ||
|| [crm.activity.binding.bind](./crm-activity-binding-bind-rest-v3.md) | Binds an activity to a CRM item ||
|| [crm.activity.binding.unbind](./crm-activity-binding-unbind-rest-v3.md) | Unbinds an activity from a CRM item ||
|| [crm.activity.binding.move](./crm-activity-binding-move-rest-v3.md) | Moves an activity binding to another CRM item ||
|| [crm.activity.binding.list](./crm-activity-binding-list-rest-v3.md) | Returns activity bindings ||
|| `crm.activity.binding.field.list` | Returns the description of binding fields ||
|| `crm.activity.binding.field.get` | Returns the description of a binding field by name ||
|#

### Emails

#|
|| **Method** | **Description** ||
|| [crm.activity.mail.getContent](./crm-activity-mail-get-content-rest-v3.md) | Returns the email text and recipients ||
|| [crm.activity.mail.getThread](./crm-activity-mail-get-thread-rest-v3.md) | Returns an email thread ||
|| [crm.activity.mail.reply](./crm-activity-mail-reply-rest-v3.md) | Sends a reply to an email ||
|#
