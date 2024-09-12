# Add Custom Deal Type crm.activity.type.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing
- links to yet-to-be-created pages (configurable deals) are not provided

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.type.add` registers its own subtype of deals by specifying a name and an icon.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **TYPE_ID**
[`unknown`](../../../../data-types.md) | Provider deal type (when creating a deal this is PROVIDER_TYPE_ID) | ||
|| **NAME**
[`unknown`](../../../../data-types.md) | The name of your deal type | ||
|| **ICON_FILE**
[`unknown`](../../../../data-types.md) | The icon file for your deal type | ||
|| **IS_CONFIGURABLE_TYPE**
[`unknown`](../../../../data-types.md) | Default value - `N`. Value `Y` indicates that the type will be used for [configurable deals](.). | ||
|#

## Examples

```js
BX24.callMethod(
    'crm.activity.type.add',
    {
        fields:
            {
                "TYPE_ID": '1C',
                "NAME": "Deal 1C",
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

After this, it is enough to specify your type when creating a deal, and the icon and name will be loaded automatically.

```js
BX24.callMethod(
    'crm.activity.add',
    {
        fields:
            {
                "OWNER_TYPE_ID": 1,
                "OWNER_ID": selectedEntityId,
                "PROVIDER_ID": 'REST_APP',
                "PROVIDER_TYPE_ID": '1C',
                "SUBJECT": "New Deal",
                "COMPLETED": "N",
                "RESPONSIBLE_ID": 1,
                "DESCRIPTION": "Description of the new deal"
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

{% include [Footnote on examples](../../../../../_includes/examples.md) %}