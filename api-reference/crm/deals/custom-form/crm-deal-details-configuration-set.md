# Set Parameters for the CRM Deal Detail Card `crm.deal.details.configuration.set`

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

The method `crm.deal.details.configuration.set` allows you to set the settings for deal cards. The method records personal settings for the specified user or general settings for all users.

{% note warning %}

Please note that the settings for deal cards of different categories (or funnels) may differ from each other. 
To switch between settings for deal cards of different categories, the parameter **dealCategoryId** is used.

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **scope**
[`unknown`](../../../data-types.md) | The scope of the settings. Allowed values:

- **P** - personal settings,
- **C** - general settings.
 ||
|| **userId**
[`unknown`](../../../data-types.md) | User identifier. If not specified, the current user is taken. Needed only when setting personal settings. ||
|| **extras**
[`unknown`](../../../data-types.md) | Additional parameters. Here, the parameter `dealCategoryId` can be specified for deals. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    //---
    //Setting personal settings for general deal cards for the user with identifier 1.
    BX24.callMethod(
        "crm.deal.details.configuration.set",
        {
            scope: "P",
            userId: 1,
            data:
            [
                {
                    name: "main",
                    title: "About the Deal",
                    type: "section",
                    elements:
                    [
                        { name: "TITLE" },
                        { name: "OPPORTUNITY_WITH_CURRENCY" },
                        { name: "STAGE_ID" },
                        { name: "BEGINDATE" },
                        { name: "CLOSEDATE" },
                        { name: "CLIENT" }
                    ]
                },
                {
                    name: "additional",
                    title: "Additional Information",
                    type: "section",
                    elements:
                    [
                        { name: "TYPE_ID" },
                        { name: "SOURCE_ID" },
                        { name: "SOURCE_DESCRIPTION" },
                        { name: "OPENED" },
                        { name: "ASSIGNED_BY_ID" },
                        { name: "OBSERVER" },
                        { name: "COMMENTS" }
                    ]
                },
                {
                    name: "products",
                    title: "Products",
                    type: "section",
                    elements:
                    [
                        { name: "PRODUCT_ROW_SUMMARY" }
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

{% include [Examples Note](../../../../_includes/examples.md) %}