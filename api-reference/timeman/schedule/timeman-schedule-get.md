# Get Work Schedule timeman.schedule.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameter specifications are missing
- examples are absent
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.schedule.get` allows you to retrieve the work schedule by its identifier.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **id**
[`int`](../../data-types.md) | Schedule identifier | ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "timeman.schedule.get",
        {
            id: 2
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

{% include [Note on examples](../../../_includes/examples.md) %}