# Get access permissions of the current user landing.site.getRights

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- corrections needed for standard writing
- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.getRights` will return the permissions of the current user. In the case of a non-existent site or lack of permissions for it, the same state will be returned – an empty array. Otherwise, an array consisting of possible values will be returned:

- **denied** - all denied,
- **read** – read (permission is automatically granted by the system when any other value except denied is specified),
- **edit** – edit (content of pages),
- **sett** – change settings,
- **public** – publish,
- **delete** – delete (to trash, and restore from trash).

## Parameters

#|
|| **Method** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Site identifier. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.site.getRights',
        {
            id: 645
        },
        function(result)
        {
            if(result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}