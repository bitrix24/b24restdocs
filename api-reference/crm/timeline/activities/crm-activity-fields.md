# Get a list of fields for crm.activity.fields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.fields` returns the description of activity fields.

## Parameters

No parameters

## Examples

```js
BX24.callMethod(
    "crm.activity.fields",
    {},
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    }
);
```

{% include [Examples note](../../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** | **Note** ||
|| **ASSOCIATED_ENTITY_ID**
[`integer`](../../../data-types.md) | Identifier of the entity associated with the deal | Read-only ||
|| **AUTHOR_ID**
[`user`](../../../data-types.md)
| Creator of the deal | ||
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
[`crm.enum.activitydirection`](../../../data-types.md) | Direction of the deal: incoming/outgoing. | Relevant for calls and e-mails, not used for meetings. ||
|| **EDITOR_ID**
[`user`](../../../data-types.md) | Who modified | Read-only ||
|| **END_TIME**
[`datetime`](../../../data-types.md) | End time | ||
|| **FILES**
[`diskfile`](../../../data-types.md) | Attached files | Multiple ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the deal | Read-only ||
|| **LAST_UPDATED**
[`datetime`](../../../data-types.md) | Date of last update | Read-only ||
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
[`string`](../../../data-types.md) | Identifier of the provider | Read-only ||
|| **PROVIDER_TYPE_ID**
[`string`](../../../data-types.md) | Identifier of the provider type | Status from the directory ||
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
[`diskfile`](../../../data-types.md) | Attached files | Multiple. Deprecated, kept for compatibility. ||
|#