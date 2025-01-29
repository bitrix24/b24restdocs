# Get the enumeration elements "Activity Direction" crm.enum.activitydirection

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no response in case of error
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```js
crm.enum.activitydirection()
```

Returns the enumeration elements "Activity Direction" (for emails and calls). Values: 1 - Incoming, 2 - Outgoing.

## Parameters

No parameters.

## Examples

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        "crm.enum.activitydirection",
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

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "ID": 0,
            "NAME": "",
            "SYMBOL_CODE": "",
            "SYMBOL_CODE_SHORT": ""
        },
        {
            "ID": 1,
            "NAME": "Incoming",
            "SYMBOL_CODE": "",
            "SYMBOL_CODE_SHORT": ""
        },
        {
            "ID": 2,
            "NAME": "Outgoing",
            "SYMBOL_CODE": "",
            "SYMBOL_CODE_SHORT": ""
        }
    ],
    "time": {
        "start": 1737527595.9598,
        "finish": 1737527595.9931,
        "duration": 0.033305883407593,
        "processing": 0.0027921199798584,
        "date_start": "2025-01-22T09:33:15+01:00",
        "date_finish": "2025-01-22T09:33:15+01:00",
        "operating_reset_at": 1737528195,
        "operating": 0
    },
    "code": 200
}
```

{% include [Note on examples](../../../../_includes/examples.md) %}