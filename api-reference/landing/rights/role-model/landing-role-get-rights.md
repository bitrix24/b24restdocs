# Get a list of sites with rights for role landing.role.getRights

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

{% note info "" %}

**Scope**: [`landing`](../../../scopes/permissions.md) | **Execution Rights**: `administrator`

{% endnote %}

The method `landing.role.getRights` allows you to retrieve a list of sites for which rights are set within the role. The method will return an array (see example), where the keys will be the site identifiers, and the values will be an array of available operations (the zero key indicates default access for the role):

- **denied** - all access denied,
- **read** – read (this right is automatically granted by the system when any other right besides denied is specified),
- **edit** – edit (page content),
- **sett** – change settings,
- **public** – publish,
- **delete** – delete (to trash, and restore from trash).

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Role identifier. ||
|#

## Examples

```js
BX24.callMethod(
    'landing.role.getRights',
    {
        id: 11
    },
    function(result)
    {
        if(result.error())
        {
            console.error(result.error());
        }
        else
        {
            console.info(result.data());
        }
    }
);
```

{% include [Example footnote](../../../../_includes/examples.md) %}