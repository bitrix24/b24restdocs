# ONAPPUPDATE Application Update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing
- parameter types are not specified
- parameter requirements are not specified

{% endnote %}

{% endif %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `ONAPPUPDATE` event for application updates.

#|
|| **Field** | **Description** | **Revision** ||
|| **data** 
[`unknown`](../../data-types.md) | Array with data | ||
|| **auth** 
[`unknown`](../../data-types.md) | Authorization parameters and information about the account where the event occurred | ||
|#

## data

#|
|| **Field** | **Description** | **Revision** ||
|| **LANGUAGE_ID** 
[`unknown`](../../data-types.md) | Base language of the account | ||
|| **VERSION** 
[`unknown`](../../data-types.md) | New version of the application | ||
|| **PREVIOUS_VERSION** 
[`unknown`](../../data-types.md) | Old version of the application | ||
|#

## auth

#|
|| **Field** | **Description** | **Revision** ||
|| **access_token** 
[`unknown`](../../data-types.md) | Key for sending requests to the REST service | ||
|| **scope** 
[`unknown`](../../data-types.md) | Allowed access levels | ||
|| **domain** 
[`unknown`](../../data-types.md) | Domain of the Bitrix24 account where the application was installed | ||
|| **application_token** 
[`unknown`](../../data-types.md) | Application token, helps you "filter" unnecessary requests to the event handler; this field is present in all events | ||
|| **expires_in** 
[`unknown`](../../data-types.md) | Token expiration time, after which a new one will need to be requested | ||
|| **member_id** 
[`unknown`](../../data-types.md) | Unique identifier of the account, needed for extending authorization | ||
|| **refresh_token** 
[`unknown`](../../data-types.md) | Key for extending authorization | ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    [data] => Array(
        [LANGUAGE_ID] = de
        [VERSION] = 2
        [PREVIOUS_VERSION] => 1
    )
    [auth] => Array(
        [access_token] => lh8ze36o8ulgrljbyscr36c7ay5sinva
        [scope] => imbot
        [domain] => b24.hazz
        [application_token] => c917d38f6bdb84e9d9e0bfe9d585be73
        [expires_in] => 3600
        [member_id] => d41d8cd98f00b204e9800998ecf8427e
        [refresh_token] => 5f1ih5tsnsb11sc5heg3kp4ywqnjhd09
    )
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}