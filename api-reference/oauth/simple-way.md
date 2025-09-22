# Simplified Method for Obtaining OAuth 2.0 Tokens

The simplest scenario for accessing the REST API is when the application operates within the Bitrix24 interface. In this case, all necessary authorization data is provided to the application upon opening, and there is also the option to use the [js library](../../sdk/bx24-js-sdk/index.md) for making API calls.

The application receives the following array of POST data:

```js
array (
    'DOMAIN' => 'account.bitrix24.com',
    'PROTOCOL' => '1',
    'LANG' => 'en',
    'APP_SID' => 'dd8cec11e347088fe87c44870a9f1dba',
    'AUTH_ID' => 'ahodg4h37n89vo17gbkgq0x1l825nnb5',
    'AUTH_EXPIRES' => '3600',
    'REFRESH_ID' => '2lg086mxijlpvwh0h7r4nl19udm4try5',
    'member_id' => 'a223c6b3710f85df22e9377d6c4f7553',
    'status' => 'P'
)
```

#|
|| **Parameter** | **Description** ||
|| **DOMAIN** | Account domain ||
|| **PROTOCOL** | Access protocol:
- `0` — http
- `1` — https
||
|| **LANG** | Current language in which the user is viewing the account ||
|| **APP_SID** | Service parameter for linking the js library with the application environment ||
|| **AUTH_ID** | Main authorization token required for accessing the REST API ||
|| **AUTH_EXPIRES** | Lifetime of the authorization token ||
|| **REFRESH_ID** | Additional authorization token used to extend the saved authorization ||
|| **member_id** | Unique identifier of the account, independent of the domain name ||
|| **status** | Status of the application on the account. The value of this field is purely informational; to obtain a trusted value, use the method `oauth.bitrix.info/rest/app.info` ||
|#

Using the value of the **AUTH_ID** parameter, you can immediately make requests to the API.

As mentioned above, applications in the interface can make API requests on the client side, that is, the user's browser, by connecting the js library and using the methods [BX24.callMethod](../../sdk/bx24-js-sdk/how-to-call-rest-methods/bx24-call-method.md) and [BX24.callBatch](../../sdk/bx24-js-sdk/how-to-call-rest-methods/bx24-call-batch.md). In this case, the authorization process will occur automatically.

Thus, a simple scenario for obtaining user authorization tokens upon application installation becomes available. As you know (this was demonstrated in the [relevant example](../app-installation/local-apps/installation-master.md)), public mass-market solutions have the option to specify a separate installation script, which will be shown to the user installing your solution in a frame once — at the time of installation. In this frame, Bitrix24 transmits the same data in the POST request as in the usual case. Therefore, you can design the installation script to save tokens (most importantly, the `refresh_token` as well) on your application's side to later implement the scenario for automatic token renewal.

## Application Installation Event

{% note warning %}

This method is not reliable, as event handlers may trigger with a delay. It is recommended to use it in conjunction with one of the previously described methods if you need authorization tokens immediately after your application is installed without delay.

{% endnote %}

For applications that do not have a page in the Bitrix24 interface (the **Uses only API** parameter in the application editing form in the partner account or on the account), the following method of authorization is available. In the editing form of the version (or in the editing form of the local application on the account), you can specify the value for the **Callback link for the installation event** parameter. If you specify a link to a handler on the application side in this field, then upon application installation, the handler will receive a POST request with the following data (`application/x-www-form-urlencoded`):

```js
array(
    'event' => 'ONAPPINSTALL',
    'data' => array(
        'VERSION' => '1',
        'LANGUAGE_ID' => 'en',
    ),
    'ts' => '1466439714',
    'auth' => array(
        'access_token' => 's6p6eclrvim6da22ft9ch94ekreb52lv',
        'expires_in' => '3600',
        'scope' => 'entity,im',
        'domain' => 'account.bitrix24.com',
        'server_endpoint' => 'https://oauth.bitrix.info/rest/',
        'status' => 'F',
        'client_endpoint' => 'https://account.bitrix24.com/rest/',
        'member_id' => 'a223c6b3710f85df22e9377d6c4f7553',
        'refresh_token' => '4s386p3q0tr8dy89xvmt96234v3dljg8',
        'application_token' => '51856fefc120afa4b628cc82d3935cce',
    ),
)
```

The request fields contain information about the event (**event**), event data (**data**), and authorization data for accessing the REST API (**auth**) on behalf of the user who installed the application.

**Significant parameters:**

- **access_token** — main authorization token required for accessing the REST API
- **refresh_token** — additional authorization token used to extend the saved authorization
- **client_endpoint** — address of the account's REST interface
- **server_endpoint** — address of the server's REST interface
- **status** — status of the application on the account

For more detailed information on working with events in Bitrix24, you can refer to the [Events](../events/index.md) section.

{% note info %}

Typically, the authorization data passed to event handlers does not contain data for renewing authorization (**refresh_token**). Some important events, such as the application installation event **ONAPPINSTALL**, are exceptions.

{% endnote %}