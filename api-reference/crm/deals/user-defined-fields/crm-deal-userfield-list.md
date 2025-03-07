# Get a list of custom fields for deals crm.deal.userfield.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples (in other languages) are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.userfield.list` returns a list of custom fields for deals based on the filter.

#| 
|| **Parameter** | **Description** ||
|| **order** | Sorting fields. ||
|| **filter** | Filter fields. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.deal.userfield.list",
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

How to get a list of fields with names:

{% list tabs %}

- JS

    ```js
    crm.deal.userfield.list
    {
        order: { "SORT": "ASC" },
        filter: { LANG: 'de' }
    }
    ```

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}

## Continue your exploration

- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-precision-to-user-field.md)