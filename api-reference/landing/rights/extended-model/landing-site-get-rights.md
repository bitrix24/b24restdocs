# Get Access Permissions of the Current User landing.site.getRights

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not to be deployed to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.getRights` will return the permissions of the current user. In the case of a non-existent site or lack of permissions for it, the same state will be returned – an empty array. Otherwise, an array consisting of possible values will be returned:

- **denied** - all denied,
- **read** – read (this permission is automatically granted by the system when any other value except denied is specified),
- **edit** – edit (content of pages),
- **sett** – change settings,
- **public** – publish,
- **delete** – delete (to the trash, and restore from the trash).

## Parameters

#|
|| **Method** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Site identifier. ||
|#

## Examples

```js
BX24.callMethod(
    'landing.site.getRights',
    {
        id: 645
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

{% include [Footnote on examples](../../../../_includes/examples.md) %}