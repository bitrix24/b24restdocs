# Add Recipients to the News Feed Message log.blogpost.share

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing
  
{% endnote %}

{% endif %}

> Scope: [`log`](../scopes/permissions.md)
>
> Who can execute the method: any user

Adds recipients to a News Feed message.

## Function Parameters

#|
|| **Parameter** | **Description** ||
|| **USER_ID** | The identifier of the user adding new recipients. Only a user with administrative rights can specify a value other than the current user, and only in the on-premise version of Bitrix24. In the cloud version, this parameter can only be set to the identifier of the [current user](../how-to-call-rest-api/authorization.md#current-user-concept). ||
|| **POST_ID** | ID of the **News Feed** message. ||
|| **DEST** | Access permissions for viewing the message. Possible values include: 
 - **`SG<X>`** - workgroup with ID=X;
 - **`U<X>`** - user with ID=X;
 - **`DR<X>`** - department with ID=X, including subdivisions;
 - **`UA`** - all authorized users. ||
|#

{% include [Parameter Notes](../../_includes/required.md) %}