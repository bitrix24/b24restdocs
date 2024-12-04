# Update an existing CRM directory item crm.status.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

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

This method updates an existing directory item.

#|
|| **Parameter** | **Description** ||
|| **id^*^** | Identifier of the directory item. ||
|| **fields^*^** | Set of fields - an array of the form `array("field to update"=>"value"[, ...])`, where "field to update" can take values returned by the method [crm.status.fields](crm-status-fields.md) ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

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

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}