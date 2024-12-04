# Set Role Permissions for Site Lists landing.role.setRights

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- corrections needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

{% note info "" %}

**Scope**: [`landing`](../../../scopes/permissions.md) | **Execution Rights**: `administrator`

{% endnote %}

The method `landing.role.setRights` sets the necessary permissions within a role for site lists. All other sites not specified in the incoming array are considered unlinked from the role.

The keys of the array are the site identifiers, and the values are arrays of available operations (a zero key means default access for the role):

- **denied** - everything is prohibited,
- **read** – read (the right is automatically granted by the system when any other value except denied is specified),
- **edit** – modification (of page content),
- **sett** – change settings,
- **public** – publication,
- **delete** – deletion (to the trash, and restoration from the trash).

## Parameters

#|
|| **Parameters** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Role identifier. ||
|| **rights**
[`unknown`](../../../data-types.md) | Array of sites for linking permissions. See example. ||
|| **additional**
[`unknown`](../../../data-types.md) | Optionally, an array with additional permissions can be passed, specifying who is allowed within the role:
- **menu24** – whether to show the "Sites" / "Stores" menu item in cloud Bitrix24 for this role
- **create** – whether to allow creating sites within the role ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.role.setRights',
        {
            id: 11,
            rights: {
                '0': ['read'],
                '66': ['read','edit','sett']
            },
            additional: ['menu24', 'create']
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

{% endlist %}

{% include [Footnote on examples](../../../../_includes/examples.md) %}