# Add Estimate crm.quote.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- revisions needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.quote.add` creates a new estimate. If you need to specify any details about the buyer/seller (as there may be several for a company), use the method [crm.requisite.link.register](../requisites/links/crm-requisite-link-register.md).

The created estimate must include the seller and buyer companies:
- `COMPANY_ID` if the buyer is a company or `CONTACT_ID` if the buyer is a contact.
- `MYCOMPANY_ID` - seller. 

The identifiers specified in **crm.requisite.link.register** and in the created estimate must correspond to the buyer and seller.

#|
||  **Parameter** / **Type**| **Description** ||
|| **fields**
[`unknown`](../../data-types.md) | Set of fields - an array of the form `array("field"=>"value"[, ...])`, containing the values of the estimate fields, where "field" can take values returned by the method [crm.quote.fields](./crm-quote-fields.md).

{% note info %}

To find out the required format of the fields, execute the method [crm.quote.fields](./crm-quote-fields.md) and check the format of the returned values for these fields. 

{% endnote %}

||
|#

## Example

```js
BX24.callMethod(
    "crm.quote.add",
    {
        fields:
        {
            "TITLE": "Draft",
            "STATUS_ID": "DRAFT",
            "OPENED": "Y",
            "ASSIGNED_BY_ID": 1,
            "CURRENCY_ID": "USD",
            "OPPORTUNITY": 5000,
            "COMPANY_ID": 1,
            "COMMENTS": "New estimate.",
            "BEGINDATE": "2016-03-01T12:00:00",
            "CLOSEDATE": "2016-04-01T12:00:00"
        }
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.info("Estimate created with ID " + result.data());
    }
);
```

{% include [Footnote on examples](../../../_includes/examples.md) %}