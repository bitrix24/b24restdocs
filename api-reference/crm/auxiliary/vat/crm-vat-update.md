# Update Existing VAT Rate crm.vat.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the requirement and type of parameters are not specified
- no response in case of error 
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
crm.vat.update(id, fields)
```

This method updates an existing VAT rate.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id** | Identifier of the VAT rate. ||
|| **fields** | Set of fields - an array of the form `array("field_to_update"=>"value"[, ...])`, where "field_to_update" can take values from the method [crm.vat.fields](crm-vat-fields.md). ||
|#

{% include [Notes on parameters](../../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS
  
    ```javascript
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.vat.update",
        {
            "id": id,
            "fields":
            {
                "ACTIVE": "N"
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


{% include [Notes on examples](../../../../_includes/examples.md) %}