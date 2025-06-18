{% include notitle [Note on required parameters](required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **access_token**
[`string`](/api-reference/data-types.html) | Authorization token [OAuth 2.0](/api-reference/oauth/index.html) ||
|| **expires_in**
[`integer`](/api-reference/data-types.html) | Time in seconds until the token expires ||
|| **scope***
[`string`](/api-reference/data-types.html) | [Scope](/api-reference/scopes/permissions.html) under which the event occurred ||
|| **domain***
[`string`](/api-reference/data-types.html) | Address of Bitrix24 where the event occurred ||
|| **server_endpoint***
[`string`](/api-reference/data-types.html) | Address of the Bitrix24 authorization server needed to refresh OAuth 2.0 tokens ||
|| **status***
[`string`](/api-reference/data-types.html) | Status of the application that subscribed to this event:

- `L` — [local](/local-integrations/local-apps.html) application
- `F` — [free mass-market](/market/index.html) application

||
|| **client_endpoint***
[`string`](/api-reference/data-types.html) | Common path for REST API method calls for Bitrix24 where the event occurred ||
|| **member_id***
[`string`](/api-reference/data-types.html) | Identifier of Bitrix24 where the event occurred ||
|| **refresh_token**
[`string`](/api-reference/data-types.html) | Token for extending authorization [OAuth 2.0](/api-reference/oauth/index.html) ||
|| **application_token***
[`string`](/api-reference/data-types.html) | Token for secure event handling ||
|#

{% note alert "" %}

Authorization tokens are not always passed to the event handler. If the hit that initiated the event could not be linked to a specific Bitrix24 user, the tokens are not passed. Always check the contents of the auth key in the code.

It is recommended to store tokens obtained earlier during the application installation. Use them when working with the application interface in the form of embeds, widgets, and so on.

{% endnote %}