# Set Parameters for the CRM Lead Detail Card `crm.lead.details.configuration.set`

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.lead.details.configuration.set` sets the settings for lead cards. This method records personal settings for the specified user’s card or general settings for all users.

{% note warning %}

Please note that the settings for repeat leads may differ from those for simple leads. The parameter **leadCustomerType** is used to switch between lead card settings.

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **scope**
[`unknown`](../../../data-types.md) | The scope of the settings. Acceptable values:

- **P** - personal settings,
- **C** - general settings.
 ||
|| **userId**
[`unknown`](../../../data-types.md) | User identifier. If not specified, the current user is used. Required only when setting personal settings. ||
|| **extras**
[`unknown`](../../../data-types.md) | Additional parameters. Here, for leads, the parameter `leadCustomerType` can be specified, with acceptable values:

- **1** - simple leads,
- **2** - repeat leads.
 ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    //---
    //Setting personal lead card settings for the user with identifier 1.
    BX24.callMethod(
        "crm.lead.details.configuration.set",
        {
            scope: "P",
            userId: 1,
            data:
            [
                {
                    name: "main",
                    title: "General Information",
                    type: "section",
                    elements:
                    [
                        { name: "TITLE" },
                        { name: "STATUS_ID" },
                        { name: "NAME" },
                        { name: "BIRTHDATE" },
                        { name: "POST" },
                        { name: "PHONE" },
                        { name: "EMAIL" }
                    ]
                },
                {
                    name: "additional",
                    title: "Additional Information",
                    type: "section",
                    elements:
                    [
                        { name: "SOURCE_ID" },
                        { name: "SOURCE_DESCRIPTION" },
                        { name: "OPENED" },
                        { name: "ASSIGNED_BY_ID" },
                        { name: "OBSERVER" },
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

{% endlist %}

{% include [Footnote on examples](../../../../_includes/examples.md) %}