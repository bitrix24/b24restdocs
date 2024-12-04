# Add Custom CRM Activity Type crm.activity.type.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples missing
- success response not provided
- error response not provided
- links to yet-to-be-created pages (configurable activities) not documented

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.type.add` registers its own subtype of activities by specifying a name and an icon.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **TYPE_ID**
[`unknown`](../../../../data-types.md) | Provider activity type (when creating an activity this is PROVIDER_TYPE_ID) | ||
|| **NAME**
[`unknown`](../../../../data-types.md) | Name of your activity type | ||
|| **ICON_FILE**
[`unknown`](../../../../data-types.md) | Icon file for your activity type | ||
|| **IS_CONFIGURABLE_TYPE**
[`unknown`](../../../../data-types.md) | Default value - `N`. Value `Y` indicates that the type will be used for [configurable activities](.). | ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'crm.activity.type.add',
        {
            fields:
                {
                    "TYPE_ID": 'QuickBooks and other similar platforms',
                    "NAME": "Activity QuickBooks",
                    'ICON_FILE': document.getElementById('type-icon'), // file input node
                    "IS_CONFIGURABLE_TYPE": "N"
                }
        },
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
            {
                alert("Success: " + result.data());
            }
        }
    );
    ```

    After this, it is enough to specify your type when creating an activity, and the icon and name will be loaded automatically.

    ```js
    BX24.callMethod(
        'crm.activity.add',
        {
            fields:
                {
                    "OWNER_TYPE_ID": 1,
                    "OWNER_ID": selectedEntityId,
                    "PROVIDER_ID": 'REST_APP',
                    "PROVIDER_TYPE_ID": 'QuickBooks and other similar platforms',
                    "SUBJECT": "New activity",
                    "COMPLETED": "N",
                    "RESPONSIBLE_ID": 1,
                    "DESCRIPTION": "Description of the new activity"
                }
        },
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
            {
                alert("Success: " + result.data());
            }
        }
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../../../../_includes/examples.md) %}