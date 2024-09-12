# Get Description of Multiple Fields crm.multifield.fields

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

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

```http
crm.multifield.fields()
```

Returns the description of multiple fields. Multiple fields are used to store phone numbers, email addresses, and other contact information. In leads, contacts, and companies, the fields of this type are PHONE, EMAIL, WEB, and IM.

## Parameters

No parameters.

## Example

```json
{'LEAD': [1, 2, 3], 'CONTACT': [4, 5, 6], 'COMPANY': [7, 8, 9]}
```

## Example of searching for a contact by phone:

```javascript
BX24.callMethod(
    "crm.multifield.fields",
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