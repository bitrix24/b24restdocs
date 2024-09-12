# Merge Duplicates crm.entity.mergeBatch

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- missing examples (in other languages)
- missing success response
- missing error response
- required parameters not specified

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
crm.entity.mergeBatch({params: {entityTypeId: number, entityIds: number[]}})
```
`params` - associative array containing the keys:

#|
|| **Key** | **Description** ||
|| **entityTypeId**
| Identifier of the entity type. Can take values `1` (lead), `2` (deal), `3` (contact), or `4` (company) ||
|| **entityIds** 
| Array of identifiers of the elements to be merged ||
|#

The method will return an associative array of the following format:

```json
{
    "STATUS": "SUCCESS",
    "ENTITY_IDS": [
        "1"
    ]
}
```

Here:

#|
|| **Parameter** | **Description** ||
|| **STATUS**
| Can take values:
- `SUCCESS` - the merge was successful;
- `CONFLICT` - a conflict occurred during the merge;
- `ERROR` - an error occurred during the merge. For example, if the current user does not have permission to modify or delete records. ||
|| **ENTITY_IDS**
| Contains the identifiers of the elements that were merged, excluding the identifier of the element that remains after the merge. ||
|#

## Example Call:

```js
BX24.callMethod(
    'crm.entity.mergeBatch',
    {
        params: {
            entityTypeId: 3,
            entityIds: [1, 2, 3],
        }
    },
    (result) => {
        console.log(result);
    }
);
```

## Manual Merge for Cases Where a Conflict Occurs

To continue merging manually, in the familiar **Bitrix24** interface, simply redirect the user to the manual merge section via the corresponding link:

- Contacts: `/crm/contact/merge/?id=1,2,3`
- Companies: `/crm/company/merge/?id=1,2,3`
- Leads: `/crm/lead/merge/?id=1,2,3`
- Deals: `/crm/deal/merge/?id=1,2,3`

where the `id` parameter contains the specified identifiers of the records to be merged.

Returns identifiers of leads, contacts, and companies containing phone numbers or email addresses from the specified list.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **type^*^** | Type of communication:
- **EMAIL** - email address;
- **PHONE** - phone. ||
|| **values^*^** | Array of emails or phone numbers (up to [20 values](*value_key)). The method returns no more than 20 duplicates per entity, and not 20 new ones, but 20 old ones.

If there are 20 or more duplicates in the entity, results for the other entities will not be returned. For example, if **entity_type** is not specified and duplicates are expected across all three entities, but we have 20 or more duplicates in leads, the contact and company entities will not be returned. If the contact entity has 20 or more duplicates, we will get duplicates for leads and contacts, while the company will be absent from the selection. ||
|| **entity_type** | Can be omitted; in this case, all three types of entities will be returned. If the parameter is used, only one of them can be operated on. If an array or a non-existent parameter is specified, all types will be returned.
Entity types:
- **LEAD** - lead;
- **CONTACT** - contact;
- **COMPANY** - company. ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}

The result is returned as an object containing arrays of identifiers for leads, contacts, and companies.

Access to the array of identifiers is done by the type name.

## Example

```json
{'LEAD': [1, 2, 3], 'CONTACT': [4, 5, 6], 'COMPANY': [7, 8, 9]}
```

## Example of Searching for a Contact by Phone:

```js
// Search for a contact by phone
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

{% include [Footnote on examples](../../../_includes/examples.md) %}

[*value_key]: Limited to reduce load.