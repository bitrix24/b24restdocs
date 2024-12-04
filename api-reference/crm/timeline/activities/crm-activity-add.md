# Add System Activity crm.activity.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing
- required parameters are not specified
- examples are missing
- response in case of success is missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

Since CRM version 22.1350.0, the method is deprecated. Use the universal activity addition method [{#T}](./crm-activity-todo-add.md).

{% endnote %}

The method `crm.activity.add` creates a new activity.

## Parameters

#|
|| Parameter | Description ||
|| **fields**
[`array`](../../../data-types.md) | An array in the form array("field"=>"value"[, ...]), containing the values of the activity fields. 

{% note info %}

To find out the required format of the fields, execute the method [crm.activity.fields](./crm-activity-fields.md) and check the format of the received values for these fields.

{% endnote %}

There is an additional field `DISABLE_SENDING_MESSAGE_COPY`. It is intended to forcibly disable sending a copy of the message to the recipient from MESSAGE_FROM. If the parameter is not filled or any value other than `Y` is specified, a copy will be sent. Example:

```js
[
    'fields'=> array (
        'SETTINGS'=> array (
            'DISABLE_SENDING_MESSAGE_COPY'=>'Y'
        )
    )
]
```
 ||
|#

## Use Cases for Field Values
For email-type activities:
- if the email should not be sent, set `DIRECTION=2` and `COMPLETED='N'`;
- if it is necessary to mark emails as completed, perform an update with the completion flag set.

## Examples

{% list tabs %}

- JS

    ```js
    var paddatepart = function(part)
    {
        return part >= 10 ? part.toString() : '0' + part.toString();
    };

    var d = new Date();
    d.setDate(d.getDate() + 7);
    d.setSeconds(0);
    var dateStr = d.getFullYear() + '-' + paddatepart(1 + d.getMonth()) + '-' + paddatepart(d.getDate()) + 'T' + paddatepart(d.getHours()) + ':' + paddatepart(d.getMinutes()) + ':' + paddatepart(d.getSeconds()) + '+00:00';

    BX24.callMethod(
        "crm.activity.add",
        {
            fields:
            {
                "OWNER_TYPE_ID": 2, //from method crm.enum.ownertype: 2 - type "deal"
                "OWNER_ID": 102, //deal id
                "TYPE_ID": 2, //from method crm.enum.activitytype
                "COMMUNICATIONS": [ { VALUE:"+18005551234", ENTITY_ID:134,ENTITY_TYPE_ID:3 } ], //where 134 - contact id, 3 - type "contact"
                "SUBJECT": "New Call",
                "START_TIME": dateStr,
                "END_TIME": dateStr,
                "COMPLETED": "N",
                "PRIORITY": 3, //from method crm.enum.activitypriority
                "RESPONSIBLE_ID": 1,
                "DESCRIPTION": "Important Call",
                "DESCRIPTION_TYPE": 3, //from method crm.enum.contenttype
                "DIRECTION": 2, // from method crm.enum.activitydirection
                "WEBDAV_ELEMENTS":
                [
                    { fileData: document.getElementById('file1') }
                ],
                "FILES":
                [
                    { fileData: document.getElementById('file1') }
                ] //after installing the disk module and converting data from webdav, you can specify FILES instead of WEBDAV_ELEMENTS
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("A new call has been created with ID " + result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}