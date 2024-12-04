# Reset the parameters of crm.lead.details.configuration.reset

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.lead.details.configuration.reset` resets the settings of lead cards. This method removes the personal settings of the specified user’s card or the general settings defined for all users.

{% note warning %}

Please note that the settings for repeat leads may differ from the settings for simple leads. The parameter **leadCustomerType** is used to switch between lead card settings.

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **scope**
[`unknown`](../../../data-types.md) | The scope of the settings. Acceptable values:

- **P** - personal settings,
- **C** - general settings.
 ||
|| **userId**
[`unknown`](../../../data-types.md) | User identifier. If not specified, the current one is used. Required only when resetting personal settings. ||
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
    //Reset personal settings of lead cards for the user with identifier 1.
    BX24.callMethod(
        "crm.lead.details.configuration.reset",
        {
            scope: "P",
            userId: 1
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