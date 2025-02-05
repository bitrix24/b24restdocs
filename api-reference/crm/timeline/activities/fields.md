# CRM Activity Fields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- not all field descriptions are present in the table
- for some fields, the type is specified as a method. How should such types be formatted and where should they reference?
- configurable activity fields do not have a single field type

{% endnote %}

{% endif %}

## System Activity Fields

#|
|| **Field** | **Description** | **Note** ||
|| **ASSOCIATED_ENTITY_ID**
[`integer`](../../../data-types.md) | Identifier of the entity associated with the activity | Read-only ||
|| **AUTHOR_ID**
[`user`](../../../data-types.md)
| Creator of the activity | ||
|| **AUTOCOMPLETE_RULE**
[`integer`](../../../data-types.md) | Autocomplete | ||
|| **BINDINGS**
[`crm_activity_binding`](../../../data-types.md) | Bindings | Multiple, read-only. ||
|| **COMMUNICATIONS**
[`crm_activity_communication`](../../../data-types.md) | | Multiple, required. ||
|| **COMPLETED**
[`char`](../../../data-types.md) | Completed | ||
|| **CREATED**
[`datetime`](../../../data-types.md) | Created | ||
|| **DEADLINE**
[`datetime`](../../../data-types.md) | Due date | The field is not set directly; the value is taken from START_TIME for calls and meetings and from END_TIME for tasks. ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | Description | ||
|| **DESCRIPTION_TYPE**
[`crm.enum.contenttype`](../../../data-types.md) | Description type | ||
|| **DIRECTION**
[`crm.enum.activitydirection`](../../../data-types.md) | Activity direction: incoming/outgoing. | Relevant for calls and e-mails, not used for meetings. ||
|| **EDITOR_ID**
[`user`](../../../data-types.md) | Who modified | Read-only ||
|| **END_TIME**
[`datetime`](../../../data-types.md) | End time | ||
|| **FILES**
[`diskfile`](../../../data-types.md) | Added files | Multiple ||
|| **ID**
[`integer`](../../../data-types.md) | Activity identifier | Read-only ||
|| **LAST_UPDATED**
[`datetime`](../../../data-types.md) | Last updated date | Read-only ||
|| **LOCATION**
[`string`](../../../data-types.md) | Location. | ||
|| **NOTIFY_TYPE**
[`crm.enum.activitynotifytype`](../../../data-types.md) | Notification type | ||
|| **NOTIFY_VALUE**
[`integer`](../../../data-types.md) | | Read-only ||
|| **ORIGINATOR_ID**
[`string`](../../../data-types.md) | Identifier of the data source | Used only for binding to an external source. ||
|| **ORIGIN_ID**
[`string`](../../../data-types.md) | Identifier of the element in the data source | Used only for binding to an external source. ||
|| **ORIGIN_VERSION**
[`string`](../../../data-types.md) | Original version | Used to protect data from accidental overwriting by an external system. If the data was imported and not modified in the external system, such data can be edited in CRM without fear that the next export will lead to data overwriting. ||
|| **OWNER_ID**
[`integer`](../../../data-types.md) | Owner | Immutable. ||
|| **OWNER_TYPE_ID**
[`crm.enum.ownertype`](../../../data-types.md) | Owner type | Immutable. ||
|| **PRIORITY**
[`crm.enum.activitypriority`](../../../data-types.md) | Priority | ||
|| **PROVIDER_DATA**
[`string`](../../../data-types.md) | | ||
|| **PROVIDER_GROUP_ID**
[`string`](../../../data-types.md) | | ||
|| **PROVIDER_ID**
[`string`](../../../data-types.md) | Provider identifier | Read-only ||
|| **PROVIDER_TYPE_ID**
[`string`](../../../data-types.md) | Provider type identifier | Status from the directory ||
|| **PROVIDER_PARAMS**
[`object`](../../../data-types.md) | | ||
|| **RESPONSIBLE_ID**
[`user`](../../../data-types.md) | Responsible person | Required. ||
|| **RESULT_CURRENCY_ID**
[`string`](../../../data-types.md) | | ||
|| **RESULT_MARK**
[`integer`](../../../data-types.md) | | ||
|| **RESULT_SOURCE_ID**
[`string`](../../../data-types.md) | | ||
|| **RESULT_STATUS**
[`integer`](../../../data-types.md) | | ||
|| **RESULT_STREAM**
[`integer`](../../../data-types.md) | Report statistics | ||
|| **RESULT_SUM**
[`double`](../../../data-types.md) | | ||
|| **RESULT_VALUE**
[`double`](../../../data-types.md) | | ||
|| **SETTINGS**
[`object`](../../../data-types.md) | Settings | ||
|| **START_TIME**
[`datetime`](../../../data-types.md) | Start time of execution | ||
|| **STATUS**
[`crm_enum_activitystatus`](../../../data-types.md) | Status | ||
|| **SUBJECT**
[`string`](../../../data-types.md) | Subject | Required ||
|| **TYPE_ID**
[`crm_enum_activitytype`](../../../data-types.md) | Type | Required, immutable ||
|| **WEBDAV_ELEMENTS**
[`diskfile`](../../../data-types.md) | Added files | Multiple. Deprecated, kept for compatibility. ||
|#

## Configurable Activity Fields

#|
|| **Field** | **Description** ||
|| **typeId**
[`unknown`](../../../data-types.md) | Type of the configurable activity. If not specified, the default value `CONFIGURABLE` is set. If specified, the value must correspond to one of the types created by the method [crm.activity.type.add](./types/crm-activity-type-add.md) with the field IS_CONFIGURABLE_TYPE equal to `Y` in the context of the same REST application. ||
|| **completed**
[`boolean`](../../../data-types.md) | Whether the activity is closed. To set the value, you can use: Y/N, 1/0, true/false. ||
|| **deadline**
[`datetime`](../../../data-types.md) | Deadline. ||
|| **pingOffsets**
[`array`](../../../data-types.md) | Array of offsets in seconds relative to the deadline, determining when to generate ping records for this activity. ||
|| **isIncomingChannel**
[`boolean`](../../../data-types.md) | Activity from an incoming channel. To set the value, you can use: Y/N, 1/0, true/false.  ||
|| **responsibleId**
[`user`](../../../data-types.md) | Responsible person. ||
|| **badgeCode**
[`string`](../../../data-types.md) | Badge code on the kanban corresponding to the activity (see [crm.activity.badge.list](./configurable/badges/crm-activity-badge-list.md)). ||
|| **originatorId**
[`string`](../../../data-types.md) | Identifier of the data source. ||
|| **originId**
[`string`](../../../data-types.md) | Identifier of the element in the data source. ||
|#