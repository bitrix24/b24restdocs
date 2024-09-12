# Update Existing CRM Entity crm.status.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter type is not specified
- examples are missing (in other languages)
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
crm.status.update(id, fields)
```

This method updates an existing entity in the directory.

#|
|| **Parameter** | **Description** ||
|| **id^*^** | Identifier of the directory entity. ||
|| **fields^*^** | Set of fields - an array of the form `array("field_to_update"=>"value"[, ...])`, where "field_to_update" can take values from the method [crm.status.fields](crm-status-fields.md) ||
|#

{% include [Notes on parameters](../../../_includes/required.md) %}

## Example

```javascript
var id = prompt("Enter ID");
BX24.callMethod(
    "crm.status.update",
    {
        id: id,
        fields:
        {
            "SORT": 75
        }                
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
        {
            console.info(result.data());                        
        }
    }
);
```

{% include [Notes on examples](../../../_includes/examples.md) %}