# Set Access Permissions on landing.site.setRights

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `landing.site.setRights` sets access permissions for the site. It will return *true* or an error. The method is available only to the administrator of the account, and in the cloud, it is also limited to paid plans.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Site identifier. ||
|| **rights**
[`unknown`](../../../data-types.md) | An object with permissions, where the keys are unique identifiers (user, department, group, ...), and the values are the allowed operations:
- **denied** – access denied
- **read** – read
- **edit** – edit (content of pages)
- **sett** – change settings
- **public** – publish
- **delete** – delete (to trash, and restore from trash)

Permissions are independent and can be granted selectively. For example, a user may have only the permission to publish without the ability to make any changes.

The following values can be used as keys:
- **SG<X>** - workgroup
- **U<X>** - user
- **DR<X>** - department, including subdivisions
- **UA** - all authorized users
- **G<X>** - user group ||
|#

## Examples

```js
BX24.callMethod(
    'landing.site.setRights',
    {
        id: 645,
        rights: {
            'U3': [
                'edit', 'delete'
            ],
            'U1': [
                'edit', 'sett'
            ]
        }
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