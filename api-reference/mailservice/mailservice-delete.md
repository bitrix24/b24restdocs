# Delete Mail Service mailservice.delete

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing parameter type descriptions
- no response examples

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly

{% endnote %}

> Scope: [`mailservice`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `mailservice.delete` removes a mail service.

## Parameters

#|
||  **Parameter** / **Type**| **Description** | **Available from** ||
|| **ID**
[`unknown`](../data-types.md) | Identifier of the mail service | ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "mailservice.delete",
        {
            'ID': 8
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

{% include [Footnote on examples](../../_includes/examples.md) %}