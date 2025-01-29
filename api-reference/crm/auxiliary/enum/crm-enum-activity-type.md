# Get Enumeration Items "Activity Type" crm.enum.activitytype

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing response in case of error
- missing response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```js
crm.enum.activitytype()
```

Returns the enumeration items for "Activity Type".

## Parameters

No parameters.

## Examples

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        "crm.enum.activitytype",
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

{% include [Footnote on examples](../../../../_includes/examples.md) %}

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
            "NAME": "Meeting",
            "SYMBOL_CODE": "",
            "SYMBOL_CODE_SHORT": ""
        },
        {
            "ID": 2,
            "NAME": "Call",
            "SYMBOL_CODE": "",
            "SYMBOL_CODE_SHORT": ""
        },
        {
            "ID": 3,
            "NAME": "Task",
            "SYMBOL_CODE": "",
            "SYMBOL_CODE_SHORT": ""
        },
        {
            "ID": 4,
            "NAME": "E-mail",
            "SYMBOL_CODE": "",
            "SYMBOL_CODE_SHORT": ""
        },
        {
            "ID": 5,
            "NAME": "Activity",
            "SYMBOL_CODE": "",
            "SYMBOL_CODE_SHORT": ""
        },
        {
            "ID": 6,
            "NAME": "User action",
            "SYMBOL_CODE": "",
            "SYMBOL_CODE_SHORT": ""
        }
    ],
    "time": {
        "start": 1737526634.82,
        "finish": 1737526634.8506,
        "duration": 0.030540943145752,
        "processing": 0.0023229122161865,
        "date_start": "2025-01-22T09:17:14+01:00",
        "date_finish": "2025-01-22T09:17:14+01:00",
        "operating_reset_at": 1737527234,
        "operating": 0
    }
}
```