# Get Leads, Contacts, and Companies with Matching Data crm.duplicate.findbycomm

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

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
crm.duplicate.findbycomm()
```

Returns the identifiers of leads, contacts, and companies containing phone numbers or email addresses from the specified list.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **type^*^** | Type of communication:
- **EMAIL** - email address;
- **PHONE** - phone number. ||
|| **values^*^** | Array of emails or phone numbers (up to [20 values](*key_values)). The method returns no more than 20 duplicates per entity, and these are not 20 new ones, but 20 existing ones.

If there are 20 or more duplicates in the entity, results for the other entities will not be returned. For example, if **entity_type** is not specified and duplicates are expected across all three entities, but we have 20 or more duplicates in leads, the contact and company entities will not be returned. If there are 20 or more duplicates in the contact entity, we will get duplicates for leads and contacts, while the company will be absent from the selection. ||
|| **entity_type** | Can be omitted; in this case, all three types of entities will be returned. If the parameter is used, only one of them can be operated on. If an array or a non-existent parameter is specified, all types will be returned.
Entity types:
- **LEAD** - lead;
- **CONTACT** - contact;
- **COMPANY** - company. ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

The result is returned as an object containing arrays of identifiers for leads, contacts, and companies.

Access to the array of identifiers is done by the type name.

## Example

```json
{'LEAD': [1, 2, 3], 'CONTACT': [4, 5, 6], 'COMPANY': [7, 8, 9]}
```

## Example of Searching for a Contact by Phone:

```js
//Searching for a contact by phone
BX24.callMethod(
    "crm.duplicate.findbycomm",
    {
        entity_type: "CONTACT",
        type: "PHONE",
        values: [ "8976543", "11223355" ],
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            {
                console.dir(result.data());
            }
    }
);
```

{% include [Example Notes](../../../_includes/examples.md) %}

[*key_values]: Limited to reduce load.