# Update Message Delivery Status messageservice.message.status.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- no response in case of success
- no response in case of error
- No examples in other languages

{% endnote %}

{% endif %}

> Scope: [`messageservice`](../scopes/permissions.md)
>
> Who can execute the method: administrator

This method allows you to set the delivery status of a specified message sent via a messaging provider.

#|
|| **Parameter** | **Description** ||
|| **CODE^*^** | Provider code.  ||
|| **MESSAGE_ID^*^** | Message identifier.  ||
|| **STATUS** | Message status. Required. Allowed statuses:
- `delivered` - delivered
- `undelivered` - not delivered
- `failed` - delivery error ||
|#

{% include [Footnote about parameters](../../_includes/required.md) %}

### Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'messageservice.message.status.update',
        {
            CODE: 'provider1',
            message_id: 1,
            status: 'delivered'
        },
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                alert("Success: " + result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}