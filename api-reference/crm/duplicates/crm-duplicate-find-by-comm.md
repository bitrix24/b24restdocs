# Get leads, contacts, and companies with matching data crm.duplicate.findbycomm

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

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
|| **values^*^** | Array of emails or phone numbers (up to [20 values](*value_key)). The method returns no more than 20 duplicates per entity, and these are not 20 new ones, but 20 old ones.

If there are 20 or more duplicates in the entity, results for the other entities will not be returned. For example, if **entity_type** is not specified and duplicates are expected across all three entities, but we have 20 or more duplicates in leads, the contact and company entities will not be returned. If the contact entity has 20 or more duplicates, we will get duplicates for leads and contacts, while the company will be absent from the selection. ||
|| **entity_type** | Can be omitted; in this case, all three types of entities will be returned. If the parameter is used, it can only operate with one of them. If an array or a non-existent parameter is specified, all types will be returned.
Entity types:
- **LEAD** - lead;
- **CONTACT** - contact;
- **COMPANY** - company. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

The result is returned as an object containing arrays of identifiers for leads, contacts, and companies.

Access to the array of identifiers is done by the type name.

## Example

```json
{'LEAD': [1, 2, 3], 'CONTACT': [4, 5, 6], 'COMPANY': [7, 8, 9]}
```

## Example of searching for a contact by phone:

{% list tabs %}

- JS

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

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}

[*value_key]: Limited to reduce load.