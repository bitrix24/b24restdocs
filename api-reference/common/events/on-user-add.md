# Event on User Addition onUserAdd

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `onUserAdd` event is triggered when a user is added to Bitrix24. The event occurs not after the invitation, but after the user logs into the account and completes the registration.

#|
|| **Parameter** | **Description** | **Available since** ||
|| **event**
[`unknown`](../../data-types.md) | ONUSERADD | ||
|| **data**
[`array`](../../data-types.md) | Array of user data. Array keys:
- **ID** - user identifier
- **ACTIVE** - active flag
- **EMAIL** - user's e-mail
- **NAME** - user's first name
- **LAST_NAME** - user's last name
- **PERSONAL_GENDER** - gender
- **PERSONAL_BIRTHDAY** - date of birth
- **UF_DEPARTMENT** - array of departments to which the user will be added. If the user registers in Extranet, this key is absent. | ||
|| **auth**
[`array`](../../data-types.md) | Array of authorization data | ||
|| **ts**
[`unknown`](../../data-types.md) | Timestamp of event creation | ||
|#