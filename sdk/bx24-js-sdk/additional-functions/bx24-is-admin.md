# Check User's Administrator Access BX24.isAdmin

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

```js
Boolean BX24.isAdmin()
```

The method `BX24.isAdmin` determines whether the current user has administrator rights. It only works after [BX24.init](../system-functions/bx24-init.md).

## See also

- [user.admin](../../../api-reference/common/users/user-admin.md)