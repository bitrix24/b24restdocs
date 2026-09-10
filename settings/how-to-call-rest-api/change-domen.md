# REST Call Peculiarities During Bitrix24 Address Changes

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A Bitrix24 cloud account receives a generated address in the format `b24-xxxxxx.bitrix24.com`. An administrator can replace it with another address or connect a custom domain, and the address written in the integration then stops being current.

Requests to the old address are not lost: Bitrix24 responds with a redirect — status `301` or `302` and a `Location` header with the new address. The error appears on the integration side: during a redirect, an HTTP client may repeat a `POST` request as a `GET` and lose the method parameters.

Everything below applies to a Bitrix24 cloud account. The address of an on-premise Bitrix24 is changed by the company administrator, and the redirect from the old address is configured on the web server.

Handle the redirect in your own code and take the current address from the authorization data. The procedure for changing the address on the user side is described in the article [Change Your Bitrix24 Address and Connect a Custom Domain](https://helpdesk.bitrix24.com/open/18990966/).

## What Happens to a Call After the Address Changes {#what-happens}

The behavior depends on how the method parameters are passed.

#|
|| **Parameter Passing Method** | **What Happens During a Redirect** ||
|| `GET`, parameters in the query string | The HTTP client repeats the request to the new address together with the query string. The method executes, and no difference is visible in the response ||
|| `POST`, parameters in the request body | The HTTP client may repeat the request using the `GET` method. The request body is lost: the method receives a call without parameters ||
|#

The redirect behavior is defined by the HTTP client settings, not by Bitrix24. For example, the curl library with the `CURLOPT_FOLLOWLOCATION` option enabled changes the request method to `GET` by default. A client may change the method on statuses `301` and `302`, while on `307` and `308` it must repeat the original request together with the body.

A lost body is visible in the response: the method reports missing data rather than an authorization error or an invalid address. A `crm.deal.get` call without the `id` parameter returns status `400` and an error description:

```json
{
    "error": "",
    "error_description": "ID is not defined or invalid."
}
```

If all calls stop working at once, start the check with the address: compare the address in your code with the current Bitrix24 address.

{% note info %}

The redirect after an address change is handled by [B24PySDK](../../sdk/b24pysdk/index.md): it switches to the new domain on its own, and you can subscribe to this action with a signal. For the other [Bitrix24 SDKs](../../sdk/index.md), redirect handling is not documented — check the behavior of your library.

{% endnote %}

## Where to Get the Current Address {#actual-address}

REST has no dedicated event for an address change — Bitrix24 does not notify the application about the move. The application receives the new address together with the regular authorization data.

### Application {#app}

`member_id` is a permanent Bitrix24 identifier. It is passed to the application in the authorization data and does not change when the address changes. The application uses it to recognize its Bitrix24 account after the address change, and only the address that method calls go to has to be updated.

The common path for method calls is passed in the `client_endpoint` field, for example `https://mycompany.bitrix24.com/rest/`. The field is available in two places:

- in the authorization server response when tokens are issued and refreshed — the procedure is described in the article [Full OAuth 2.0 Authorization Protocol](../oauth/index.md)
- in the `auth` parameters when the [event](../../api-reference/events/index.md) handler is called, next to the `domain` field — the address of the Bitrix24 account where the event occurred

Retain the `client_endpoint` value under the `member_id` key and overwrite it with every new set of tokens. The application then calls the address from the latest authorization data instead of the address written in the code.

### Incoming Webhook {#webhook}

The address is embedded in the webhook URL in the format `https://mycompany.bitrix24.com/rest/1/8g9l071eismy9q2l/crm.deal.add`, and an incoming webhook does not pass a new address. Take the new address in one of two ways:

- from the `Location` header in the response with the redirect. The header is available only if the client does not follow the redirect on its own — as in the [examples below](#manual)
- from the [incoming webhook](../../local-integrations/local-webhooks.md) settings. Open your webhook and copy the URL again — it already contains the new address

{% note warning %}

The webhook URL contains a secret code, `8g9l071eismy9q2l` in the example above. It grants access to Bitrix24 data within the selected `scope` and the permissions of the employee who created the webhook. Do not publish the full URL and do not write it to logs.

{% endnote %}

## How to Handle the Redirect in Your Code {#handle-redirect}

There are two approaches: handle the redirect manually or let the HTTP client repeat the `POST` automatically. Choose manual handling by default — it does not depend on the language or library and gives you the new address to retain.

### Disable the Redirect and Repeat the Request Manually {#manual}

Prevent the HTTP client from following the redirect, check the response status, take the new address from the `Location` header, and repeat the same `POST` request with the same parameters.

{% list tabs %}

- Python

    ```python
    import requests

    url = "https://mycompany.bitrix24.com/rest/1/8g9l071eismy9q2l/crm.deal.add"
    params = {"fields": {"TITLE": "New deal"}}

    response = requests.post(url, json=params, allow_redirects=False)

    if response.status_code in (301, 302):
        url = response.headers["Location"]
        response = requests.post(url, json=params, allow_redirects=False)

        endpoint = url.split("/rest/")[0] + "/rest/"
        # retain endpoint on your side

    print(response.json())
    ```

- PHP

    ```php
    $url = 'https://mycompany.bitrix24.com/rest/1/8g9l071eismy9q2l/crm.deal.add';
    $params = ['fields' => ['TITLE' => 'New deal']];

    $ch = curl_init($url);
    curl_setopt($ch, CURLOPT_POST, true);
    curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($params));
    curl_setopt($ch, CURLOPT_HTTPHEADER, ['Content-Type: application/json']);
    curl_setopt($ch, CURLOPT_FOLLOWLOCATION, false);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

    $response = curl_exec($ch);
    $statusCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);

    if ($statusCode === 301 || $statusCode === 302) {
        $url = curl_getinfo($ch, CURLINFO_REDIRECT_URL);
        curl_setopt($ch, CURLOPT_URL, $url);
        $response = curl_exec($ch);

        $endpoint = explode('/rest/', $url)[0] . '/rest/';
        // retain $endpoint on your side
    }

    curl_close($ch);

    print_r(json_decode($response, true));
    ```

{% endlist %}

The `Location` header also contains the path of the called method, so the address is used in full for the repeated request. For retention, keep only the common path up to `/rest/` — the same value the application receives in `client_endpoint`.

### Allow the POST to Be Repeated on a Redirect {#postredir}

The `CURLOPT_POSTREDIR` option specifies the response statuses on which curl repeats the request using the `POST` method instead of `GET`. The value is composed of the bit flags `CURL_REDIR_POST_301`, `CURL_REDIR_POST_302`, and `CURL_REDIR_POST_303`. The sum of the first two equals `3` — this notation is also common. The option works only together with `CURLOPT_FOLLOWLOCATION`.

```php
$url = 'https://mycompany.bitrix24.com/rest/1/8g9l071eismy9q2l/crm.deal.add';
$params = ['fields' => ['TITLE' => 'New deal']];

$ch = curl_init($url);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($params));
curl_setopt($ch, CURLOPT_HTTPHEADER, ['Content-Type: application/json']);
curl_setopt($ch, CURLOPT_FOLLOWLOCATION, true);
curl_setopt($ch, CURLOPT_POSTREDIR, CURL_REDIR_POST_301 | CURL_REDIR_POST_302);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

$response = curl_exec($ch);
curl_close($ch);

print_r(json_decode($response, true));
```

There is less code, but the integration keeps calling the old address and receives an extra redirect on every call. The new address does not arrive as a separate field — you retrieve it from the effective request address through `curl_getinfo($ch, CURLINFO_EFFECTIVE_URL)`. The option is specific to curl: in clients without such a setting, for example the `requests` library for Python, manual handling remains the only way.

## What to Check in Your Integration {#checklist}

1. The common path for calls in the format `https://mycompany.bitrix24.com/rest/` is retained in one place rather than written in several files
2. In an application, this value is updated from `client_endpoint` and retained under the `member_id` key
3. `POST` requests do not lose the body during a redirect
4. The logs show which Bitrix24 address and which method the request went to, and with which status the response came back

## Continue Learning

- [{#T}](./authorization.md) — how to pass webhook and application authorization data in a request
- [{#T}](./general-principles.md) — what the method call address consists of and how to pass parameters
- [{#T}](../oauth/index.md) — the full cycle of issuing and refreshing tokens, in which `client_endpoint` arrives
- [{#T}](../../api-reference/events/index.md) — the composition of the `auth` parameters in an event handler call
