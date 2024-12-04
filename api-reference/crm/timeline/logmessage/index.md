# Log Message Journal

The log message journal is a special type of timeline record created in the context of a specific [Rest application](https://helpdesk.bitrix24.com/examples/app.zip). It contains less important data than other records and is distinguished by a muted gray background, attracting less attention.

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: `depends on the method`

A list of methods for managing the log message journal.

{% note info "" %}

Important: The methods [`crm.timeline.logmessage.get`](./crm-timeline-logmessage-get.md) and [`crm.timeline.logmessage.list`](./crm-timeline-logmessage-list.md) only return records that were previously created using [`crm.timeline.logmessage.add`](./crm-timeline-logmessage-add.md). System records cannot be retrieved using these methods.

{% endnote %}

#|
|| **Method** | **Description** ||
|| [crm.timeline.logmessage.add](./crm-timeline-logmessage-add.md) | Adds a new log message to the timeline ||
|| [crm.timeline.logmessage.get](./crm-timeline-logmessage-get.md) | Retrieves information about a log message ||
|| [crm.timeline.logmessage.list](./crm-timeline-logmessage-list.md) | Retrieves a list of all log messages for a specific entity ||
|| [crm.timeline.logmessage.delete](./crm-timeline-logmessage-delete.md) | Deletes a log message ||
|| [crm.timeline.icon.*](./icons/index.md) | Methods for working with record icons ||
|| [crm.timeline.logo.*](./logo/index.md) | Methods for working with record logos ||
|#