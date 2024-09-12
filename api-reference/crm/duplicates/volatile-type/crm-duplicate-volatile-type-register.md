# Add a field to the duplicate search crm.duplicate.volatileType.register

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- missing response in case of success
- missing response in case of error
- required fields not specified
- no examples provided
  
{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
crm.duplicate.volatileType.register({entityTypeId: number, fieldCode: string})
```

#|
|| **Key** | **Description** ||
|| **entityTypeId**
| Identifier of the entity type. Can take values `1` (lead), `3` (contact), or `4` (company). ||
|| **fieldCode** | The code of the field for which duplicate search needs to be enabled. ||
|#

The method will return an array containing:

#|
|| **Parameter** | **Description** ||
|| **id**
| Identifier of the added duplicate type. Note that this identifier is essentially a bitmask, so it takes specific values rather than standard auto-incrementing values. ||
|#

{% include [Footnote on parameters](../../../../_includes/required.md) %}