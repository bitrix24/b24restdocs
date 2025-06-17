# Update News Feed Message log.blogpost.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

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

Updates a message in the News Feed.

## Function Parameters

#|
|| **Parameter** | **Description** ||
|| **POST_ID** | Identifier of the News Feed message. ||
|| **USER_ID** | Identifier of the message author. Only a user with administrative rights can specify a value other than the current user, and only in the on-premise version of Bitrix24. In the cloud version, this parameter can only be set to the identifier of the [current user](../how-to-call-rest-api/authorization.md#current-user-concept). ||
|| **POST_MESSAGE** | Text of the message. ||
|| **POST_TITLE** | Title of the message. ||
|| **DEST** | List of recipients who will have the right to view the message. Possible values for the array elements:

{% include notitle [message recipients](./_includes/log-recepients.md) %}

Default value - `['UA']`
||
|| **SPERM** | List of recipients who will have the right to view the message. (deprecated). Similar to `DEST` ||
|| **FILES** | Files attached to the message as an array of values. Features of working with files are described in the article [How to update and delete files](../files/how-to-update-files.md). ||
|#

{% include [Footnote on parameters](../../_includes/required.md) %}