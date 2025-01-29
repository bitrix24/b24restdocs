# Get Enumeration Items "Content Type" crm.enum.contenttype

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

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
crm.enum.contenttype()
```

Returns the enumeration items for "Content Type".

## Parameters

No parameters.

## Examples

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        "crm.enum.contenttype",
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
            "NAME": "Plain text",
            "SYMBOL_CODE": "",
            "SYMBOL_CODE_SHORT": ""
        },
        {
            "ID": 2,
            "NAME": "bbCode",
            "SYMBOL_CODE": "",
            "SYMBOL_CODE_SHORT": ""
        },
        {
            "ID": 3,
            "NAME": "HTML",
            "SYMBOL_CODE": "",
            "SYMBOL_CODE_SHORT": ""
        }
    ],
    "time": {
        "start": 1737527499.922,
        "finish": 1737527499.9578,
        "duration": 0.035794973373413,
        "processing": 0.0021491050720215,
        "date_start": "2025-01-22T09:31:39+01:00",
        "date_finish": "2025-01-22T09:31:39+01:00",
        "operating_reset_at": 1737528099,
        "operating": 0
    }
}
```