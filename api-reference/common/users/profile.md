# Get Basic Information About the Current User Profile

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- missing parameters or fields
- missing examples
- missing success response
- missing error response
- links to pages that have not yet been created are not specified

{% endnote %}

{% endif %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `profile` method allows you to retrieve basic information about the current user without any scopes, unlike user.current.

Returned fields:

```json
ADMIN true
ID "1"
LAST_NAME "Vostrikov"
NAME "Sergey"
PERSONAL_GENDER ""
PERSONAL_PHOTO "https://***.bitrix24.com/*****"
TIME_ZONE null
TIME_ZONE_OFFSET 7200
```