# Delete VAT Rate crm.vatdelete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- type and required parameter are not specified
- no response in case of error and success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
crm.vat.delete(id)
```

Deletes the VAT rate.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id** | Identifier of the VAT rate. ||
|#

{% include [Note on parameters](../../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS
  
    ```javascript
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.vat.delete",
        { "id": id },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

{% endlist %}


{% include [Note on examples](../../../../_includes/examples.md) %}