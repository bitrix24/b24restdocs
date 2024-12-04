# Update the estimate crm.quote.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing
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

The method `crm.quote.update` updates an existing estimate.


#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the estimate. ||
|| **fields**
[`unknown`](../../data-types.md) | [Set of fields](./crm-quote-add.md) - an array in the form `array("field_to_update"=>"value"[, ...])`, where "field_to_update" can take values returned by the method [crm.quote.fields](./crm-quote-fields.md). 
{% note info %}

To find out the required format of the fields, execute the method [crm.quote.fields](./crm-quote-fields.md) and check the format of the returned values for these fields. 

{% endnote %}
||
|| **params**
[`unknown`](../../data-types.md) | Set of parameters. `REGISTER_HISTORY_EVENT` - create a record in the history, default value: "Y". Additionally, a notification will be sent to the person responsible for the estimate. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.quote.update",
        {
            id: id,
            fields: { "STATUS_ID": "SENT" }    
        },
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

{% include [Footnote about examples](../../../_includes/examples.md) %}