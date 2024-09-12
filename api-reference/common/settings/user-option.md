# Get data associated with the application and user user.option.*

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing parameters or fields
- parameter types not specified
- required parameters not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The methods **user.option.*** are similar to the methods **app.option.***, but the binding is to the application and the user.

**user.option.get** is analogous to [app.option.get](./app-option-get.md).

**user.option.set** is analogous to [app.option.set](./app-option-set.md).

Depending on the type of application, it may be bound to the user who installed it or to the user with whom it interacts.