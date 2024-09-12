# Set Parameters for Individual CRM Company Detail Card Configuration

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.company.details.configuration.set` sets the configuration for company detail cards. This method records personal settings for the specified user or general settings for all users.

#|
|| **Parameter** | **Description** ||
|| **scope**
[`unknown`](../../../data-types.md) | The scope of the settings. Allowed values:

- **P** - personal settings,
- **C** - general settings.
 ||
|| **userId**
[`unknown`](../../../data-types.md) | User identifier. If not specified, the current user is used. Required only when setting personal settings. ||
|| **data**
[`unknown`](../../../data-types.md) | Array of parameters. ||
|#

## Examples

```js
//Setting personal settings for the company detail card for the user with identifier 1.
BX24.callMethod(
    "crm.company.details.configuration.set",
    {
        scope: "P",
        userId: 1,
        data:
        [
            {
                name: "main",
                title: "About the Company",
                type: "section",
                elements:
                [
                    { name: "TITLE" },
                    { name: "LOGO" },
                    { name: "COMPANY_TYPE" },
                    { name: "POST" },
                    { name: "PHONE" },
                    { name: "EMAIL" },
                    { name: "CONTACT" }
                ]
            },
            {
                name: "additional",
                title: "Additional Information",
                type: "section",
                elements:
                [
                    { name: "INDUSTRY" },
                    { name: "OPENED" },
                    { name: "ASSIGNED_BY_ID" },
                    { name: "COMMENTS" }
                ]
            }
        ]
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    }
);
//---
```

{% include [Note on Examples](../../../../_includes/examples.md) %}