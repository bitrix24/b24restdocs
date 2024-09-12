# Set Access Codes for Role landing.role.setAccessCodes

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
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

The method `landing.role.setAccessCodes` sets access codes for a role, which will apply to this role (and its restrictions on sites). The method takes the role identifier and an array of access codes as input. If the array is empty, the codes for the role will be reset.

Permissions are independent and can be granted selectively. For example, a user may have only the right to publish without any modification rights.

The following values can be used as keys:

- **SG<X>** - workgroup, for example SG1 - workgroup with identifier 2;
- **U<X>** - user, for example U45 - user with identifier 45;
- **DR<X>** - department, including subdivisions, for example DR23 - section with identifier 23;
- **UA** - all authorized users.
- **G<X>** - user group, for example G2 - user group with identifier 2.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Role identifier. ||
|| **codes**
[`unknown`](../../../data-types.md) | Array of access codes. ||
|#

## Examples

```js
BX24.callMethod(
    'landing.role.setAccessCodes',
    {
        id: 11,
        codes: [
            'SG3_A', 'G4'
        ]
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

{% include [Note on Examples](../../../../_includes/examples.md) %}