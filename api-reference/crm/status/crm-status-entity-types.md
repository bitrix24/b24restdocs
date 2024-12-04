# Get CRM Status Entity Types

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing (in other languages)
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
crm.status.entity.types()
```

The method returns a description of the entity types. The result is an array of the form `array(array("ID"=>"symbolic identifier of the directory", "NAME"=>"name of the directory")[, ...])`.

## Parameters

No parameters.

## Example

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod(
        "crm.status.entity.types",
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

{% include [Footnote on examples](../../../_includes/examples.md) %}