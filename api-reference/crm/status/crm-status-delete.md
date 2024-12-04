# Delete CRM Status Element: crm.status.delete

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
crm.status.delete(id, params)
```

This method deletes a directory element.

#|
|| **Parameter** | **Description** ||
|| **id^*^** | Identifier of the directory element. ||
|| **params** | Set of parameters. FORCED - flag for forcibly deleting system elements. Default is N. If the element being deleted is a system element, it will not be deleted. If Y is passed, the element will be deleted regardless. To delete a system element, use the second example in the description. ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```javascript
    var id = prompt("Enter the ID of the custom element");
    BX24.callMethod(
        "crm.status.delete",
        { id: id },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

    ```javascript
    var id = prompt("Enter the ID of the custom or system element");
    BX24.callMethod(
        "crm.status.delete",
        { id: id, params:{ FORCED: "Y" } },
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

{% include [Footnote on examples](../../../_includes/examples.md) %}