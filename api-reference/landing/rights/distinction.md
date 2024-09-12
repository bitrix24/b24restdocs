# Differences Between Models

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

Two models of permissions are supported: role-based and extended.

**Role-based model** implies setting permissions based on roles in the public section of Sites24 and Stores24.  
**Extended model** implies setting permissions within the settings of each site.

The models are completely separated in logic, both at the stage of setting permissions and at the stage of controlling the output of entities. This means that you can switch between models seamlessly and return back, as previous settings will be preserved.

{% note warning %}

When calling methods for different models, the current mode is not determined; this must be defined by the developer independently using the appropriate [method](./landing-role-is-enabled.md).

{% endnote %}