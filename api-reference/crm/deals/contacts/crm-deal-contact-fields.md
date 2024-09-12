# Get Fields for the Deal-Contact Connection crm.deal.contact.fields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing (in other languages)
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.contact.fields` returns the description of the fields used by the methods of the `crm.deal.contact.*` family, such as [crm.deal.contact.items.get](./crm-deal-contact-items-get.md), [crm.deal.contact.items.set](./crm-deal-contact-items-set.md), [crm.deal.contact.add](./crm-deal-contact-add.md), etc.

No parameters.

## Example

```js
BX24.callMethod(
    "crm.deal.contact.fields",
    {},
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    }
);
```

{% include [Footnote on examples](../../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting index (number). Determines the order in which linked contacts will be displayed in the deal. ||
|| **IS_PRIMARY**
[`char`](../../../data-types.md) | [Y/N] Indicates whether the link is primary. There is always a primary contact in the deal. It will be `IS_PRIMARY=Y`, while the others will be `IS_PRIMARY=N`. ||
|| **CONTACT_ID**
[`integer`](../../../data-types.md) | Identifier of the contact linked to the deal (number). ||
|#