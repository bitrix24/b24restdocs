# Create a New Company crm.company.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- required parameters are not specified
- examples are missing
- response in case of success is missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.company.add` creates a new company.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields**
[`array`](../../data-types.md) | A set of fields - an array in the form array("field"=>"value"[, ...]), containing the values of the company fields. 

{% note info %}

To find out the required format of the fields, execute the method [crm.company.fields](./crm-company-fields.md) and check the format of the received values for these fields.

{% endnote %}
 ||
|| **params**
[`array`](../../data-types.md) | A set of parameters. REGISTER_SONET_EVENT - register the event of adding a company in the live feed. A notification will also be sent to the person responsible for the company. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.company.add",
        {
            fields:
            {
                "TITLE": "IP Titov",
                "COMPANY_TYPE": "CUSTOMER",
                "INDUSTRY": "MANUFACTURING",
                "EMPLOYEES": "EMPLOYEES_2",
                "CURRENCY_ID": "USD",
                "REVENUE" : 3000000,
                "LOGO": { "fileData": document.getElementById('logo') },
                "OPENED": "Y",
                "ASSIGNED_BY_ID": 1,
                "PHONE": [ { "VALUE": "555888", "VALUE_TYPE": "WORK" } ]     
            },
            params: { "REGISTER_SONET_EVENT": "Y" }        
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Company created with ID " + result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}