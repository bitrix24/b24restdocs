# Get a list of custom fields for estimates by filter crm.quote.userfield.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameters need to be described here
- parameter types are not specified
- parameter requirements are not specified
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.quote.userfield.list` returns a list of custom fields for estimates by filter.

#|
||  **Parameter** / **Type**| **Description** ||
|| **order**
[`unknown`](../../data-types.md) | Sorting fields. ||
|| **filter**
[`unknown`](../../data-types.md) | Filtering fields. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.quote.userfield.list",
        {
            order: { "SORT": "ASC" },
            filter: { "MANDATORY": "N" }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.dir(result.data());
                if(result.more())
                    result.next();
            }
        }
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}