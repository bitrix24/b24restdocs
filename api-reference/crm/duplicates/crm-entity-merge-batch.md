# Merge Duplicates crm.entity.mergeBatch

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing (in other languages)
- success response is missing
- error response is missing
- required parameters are not specified

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
| Array of identifiers of the entities to be merged ||
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
| Can take the following values:
- `SUCCESS` - the merge was successful;
- `CONFLICT` - a conflict occurred during the merge;
- `ERROR` - an error occurred during the merge. For example, if the current user does not have permission to modify or delete records. ||
|| **ENTITY_IDS**
| Contains the identifiers of the entities that were merged, excluding the identifier of the entity that remains after the merge. ||
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

## Manual Merge for Cases Where a Conflict Occurred

To continue merging manually, in the familiar **Bitrix24** interface, simply redirect the user to the manual merge section via the corresponding link:

- Contacts: `/crm/contact/merge/?id=1,2,3`
- Companies: `/crm/company/merge/?id=1,2,3`
- Leads: `/crm/lead/merge/?id=1,2,3`
- Deals: `/crm/deal/merge/?id=1,2,3`

where the `id` parameter contains the specified identifiers of the records to be merged.

{% include [Footnote on Examples](../../../_includes/examples.md) %}