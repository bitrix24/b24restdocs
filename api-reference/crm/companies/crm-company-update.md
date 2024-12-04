# Update Existing Company crm.company.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.company.update` updates an existing company.

{% note warning %}

It is strongly recommended to pass the complete set of address fields when updating the address in the update method. The specifics of updating address fields are described [here](../data-types.md).

{% endnote %}

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Company identifier. ||
|| **fields**
[`unknown`](../../data-types.md) | [Set of fields](./crm-company-add.md) - an array of the form array("field to update"=>"value"[, ...]), where "field to update" can take values returned by the method [crm.company.fields](./crm-company-fields.md). 

{% note info %}

To find out the required format of the fields, execute the method [crm.company.fields](./crm-company-fields.md) and check the format of the returned values for these fields.

{% endnote %}

 ||
|| **params**
[`unknown`](../../data-types.md) | Set of parameters. `REGISTER_SONET_EVENT` - register the event of the company's change in the live feed. A notification will also be sent to the person responsible for the company. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.company.update",
        {
            id: id,
            fields:
            {
                "CURRENCY_ID": "USD",
                "REVENUE" : 500000,
                "EMPLOYEES": "EMPLOYEES_3"
            },
            params: { "REGISTER_SONET_EVENT": "Y" }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}