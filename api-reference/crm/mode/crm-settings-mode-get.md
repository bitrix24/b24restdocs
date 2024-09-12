# Get Current CRM Operating Mode Settings crm.settings.mode.get

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameters are not specified
- examples are missing
- response on success is absent
- response on error is absent

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
crm.settings.mode.get()
```

Returns the current CRM operating mode settings:

- 1 - classic operating mode (with leads).
- 2 - operating mode without leads.

In other words, the method `crm.settings.mode.get` returns a value defined in [crm.enum.settings.mode](../auxiliary/enum/crm-enum-settings-mode.md).