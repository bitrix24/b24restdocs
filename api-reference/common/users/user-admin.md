# Define Access Permissions for Managing Application Settings user.admin

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing
- no response in case of an error

{% endnote %}

{% endif %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `user.admin` method determines whether the current user has the permissions to manage application settings.

## Example

Request
```http
https://my.bitrix24.com/rest/user.admin.json?auth=d161f25928c3184678924ec127edd29a
```

{% include [Example Note](../../../_includes/examples.md) %}

## Successful Response

> 200 OK
```json
{"result":true}
```

## See Also

- [BX24.isAdmin](../../bx24-js-sdk/additional-functions/bx24-is-admin.md)