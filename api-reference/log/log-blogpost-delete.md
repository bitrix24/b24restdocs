# Delete News Feed Message log.blogpost.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`log`](../scopes/permissions.md)
>
> Who can execute the method: any user

Deletes the specified news feed message.

#|
|| **Parameter** | **Description** ||
|| **USER_ID** | ID of the user who is deleting the message. Only a user with administrative rights can specify a value that is not the current user, and only in the case of the on-premise version of Bitrix24. In the cloud version, this parameter can only accept the identifier of the [current user](../how-to-call-rest-api/authorization.md#concept-of-current-user).  ||
|| **POST_ID** | Numeric ID of the message. ||
|#

{% include [Notes on parameters](../../_includes/required.md) %}