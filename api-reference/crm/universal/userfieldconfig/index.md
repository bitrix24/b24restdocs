# Custom Fields

Currently, this API is available only for the [rpa](../../../outdated/rpa/index.md) and [crm](../../../crm/index.md) modules.

Physically, the controller is located in the **main** module. However, it has been extracted as a separate **scope** for REST (it can be accessed via AJAX as *main.userfieldconfig*).

{% note info "" %}

For the methods to work, the application must have access not only to the `userfieldconfig` scope but also to the scope for which the field settings are being modified. For example, to change the field settings in the Business Automation module, the application must have access to both `userfieldconfig` and `rpa`.

{% endnote %}

When calling any of the methods, it is necessary to pass the module identifier **moduleId**, which determines the user's access to the fields.
