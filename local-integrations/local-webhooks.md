# Incoming and Outgoing Webhooks

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Webhooks are suitable for local integrations in a single Bitrix24 account. An incoming webhook calls REST API methods on behalf of the employee who created it. An outgoing webhook sends a Bitrix24 event to your handler URL.

## What to Choose

#|
|| **Tool** | **What It Does** | **When It Fits** | **What to Consider** ||
|| Incoming webhook | Calls REST API methods through a secret URL | Quick integrations, tests, scripts, and data exchange with an external system | Works within the `scope` and employee permissions. Some methods require the application context and are not available through a webhook ||
|| Outgoing webhook | Sends a Bitrix24 event to your handler URL | Reacting to data changes, starting synchronization, and notifying an external system | The handler must be available from the external network. To retrieve full object data, an additional method call is usually required ||
|#

## Incoming Webhook {#incoming-webhook}

Choose an incoming webhook for internal integrations and quick checks when:

- you need to quickly test a method call in the browser, request generator, or with `curl`
- the integration works with only one Bitrix24 account
- requests must be executed on behalf of a specific employee
- you do not need an application interface, event handler, or solution installation on other Bitrix24 accounts

### How an Incoming Webhook Works

Incoming webhook permissions are defined by the employee who created it and the selected `scope` list:

- an incoming webhook can be created by an administrator or by an employee for whom the administrator has allowed webhook creation
- the Bitrix24 account must have [access to the REST API](../first-steps/access-to-rest-api.md)
- requests are executed within the [scope](../api-reference/scopes/permissions.md) selected in the webhook settings
- requests are executed with the permissions of the employee who created the webhook
- the webhook secret code is available only to the employee who created it. If an administrator edits another user's webhook, the secret code is updated and the administrator becomes the webhook owner
- some methods are not available through a webhook because they require the application context. For example, widget embedding methods such as [placement.bind](../api-reference/widgets/placement-bind.md), some [telephony](../api-reference/telephony/index.md) methods, and some [chatbot](../api-reference/chat-bots/index.md) scenarios
- the incoming webhook mechanism supports an expiration date. If the expiration date has passed, the request stops working and returns an authorization error

### How to Create an Incoming Webhook

Create webhooks in *Applications > Developer resources*. If a ready-made scenario fits your case, select it on the *Ready-made scenarios* tab and open its settings. If there is no suitable scenario, create an incoming webhook on the *Ready-made scenarios > Other* tab.

1. Open *Applications > Developer resources*
2. Go to *Ready-made scenarios > Other > Incoming webhook*
3. In the request generator, select a method and fill in parameters if required
4. Click *Execute* to test the call
5. Specify access permissions and click *Create*

In the request generator, you can select a method, view the method and parameter descriptions, fill in parameters, execute the request, and download a ready-made PHP code example.

If the *Incoming webhook* item is missing, the permission to create webhooks is disabled. Ask the administrator to [grant access to webhook creation](#webhook-access).

### Webhook URL Structure

Example URL:

```text
https://example.bitrix24.com/rest/1/xxxxxxxx/department.get.json?ID=42
```

The URL consists of several parts:

- `example.bitrix24.com` — your Bitrix24 address
- `/rest` — path to the REST API
- `/1` — identifier of the employee who created the webhook
- `/xxxxxxxx` — webhook secret code
- `/department.get` — name of the called method
- `.json` — response format. This suffix can be omitted, `json` is used by default
- `?ID=42` — method parameters

{% note warning "" %}

The webhook secret code grants access to methods within the webhook permissions. Do not pass the webhook URL to third parties or publish it in client-side code.

{% endnote %}

### How to Configure Access to Webhook Creation for Employees {#webhook-access}

An employee without administrator permissions cannot grant this access to themselves. An administrator can allow webhook creation for all employees or selected users.

1. Open *Settings > Bitrix24 settings*. The section is available only to employees with administrator permissions.
2. In the new window, go to *Security > Bitrix24 integrations*.
3. In the *Who can create incoming webhooks* field, click *Add* and select all employees or selected users

![Configure Access to Incoming Webhook Creation](../first-steps/_images/webhook.png)

{% note tip "User Documentation" %}

- [Create webhooks and apps in Bitrix24](https://helpdesk.bitrix24.com/open/21133100/)

{% endnote %}

### How to Quickly Test a Method with a GET Request

On REST API method pages, webhook calls are shown as POST requests. A GET request can be executed in the browser address bar or in the request generator when creating an incoming webhook.

This format is suitable when you need to quickly test a simple call and see how parameters look in the URL.

General URL format:

```text
https://{your-bitrix24}.bitrix24.com/rest/{user_id}/{webhook_code}/{method}.json
```

Example of a method without parameters:

```text
https://example.bitrix24.com/rest/1/xxxxxxxx/profile.json
```

Example of a method with one parameter:

```text
https://example.bitrix24.com/rest/1/xxxxxxxx/department.get.json?ID=1
```

Example of a method with an array of parameters:

```text
https://example.bitrix24.com/rest/1/xxxxxxxx/user.get.json?FILTER[ACTIVE]=true&select[]=ID&select[]=NAME
```

{% note warning "" %}

GET requests have limitations:

- GET is suitable for quick tests of simple requests
- the webhook URL can be retained in browser history, logs, and monitoring services
- for production integrations, nested parameters, files, and data changes, use a POST request
- if the URL becomes long, use a POST request, `curl`, or an SDK

{% endnote %}

### What to Check Before Production Use

Before production launch, check that:

- the required method is available for the selected `scope`
- the employee who created the webhook has permissions for the required object
- your Bitrix24 account has [access to the REST API](../first-steps/access-to-rest-api.md)
- requests are sent over HTTPS

## Outgoing Webhook {#outgoing-webhook}

Choose an outgoing webhook when:

- an external system must automatically learn about changes in Bitrix24
- synchronization must start after an object is created or changed
- it is enough to receive the event and object identifier, and detailed data can be requested with a separate method

### How an Outgoing Webhook Works

An outgoing webhook does not call a method by itself. It passes an event to your handler, and the handler decides whether an additional REST API request is required:

- you select an event and handler URL
- when the event occurs, Bitrix24 sends a POST request to this URL
- the handler receives the event name, basic object data, and service authorization fields
- if full object data is required, the handler usually calls the corresponding method through an incoming webhook or an application

### How to Create an Outgoing Webhook

1. Open *Applications > Developer resources*
2. Go to *Ready-made scenarios > Other > Outgoing webhook*
3. Specify your handler URL
4. Select the event the webhook should react to
5. Click *Create* and test the call after a test data change

The handler URL must be public and available from the external network. Do not specify `localhost`, local network addresses, or handlers with a self-signed SSL certificate.

When you create an outgoing webhook, Bitrix24 shows a token. It is required so that the handler can verify that the request actually came from your Bitrix24 account.

### What the Handler Receives

Data arrives as HTTP request parameters with the `application/x-www-form-urlencoded` type. In PHP, it is convenient to read them through `$_REQUEST`.

Example structure for the [ONCRMDEALUPDATE](../api-reference/crm/deals/events/on-crm-deal-update.md) event:

```php
Array
(
    [event] => ONCRMDEALUPDATE
    [data] => Array
        (
            [FIELDS] => Array
                (
                    [ID] => 662
                )
        )
    [ts] => 1724140800
    [auth] => Array
        (
            [domain] => example.bitrix24.com
            [member_id] => xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
            [application_token] => xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
        )
)
```

Request parameters:

#|
|| **Parameter** | **Type** | **Description** ||
|| `event` | [`string`](../api-reference/data-types.md) | Name of the event that triggered the outgoing webhook ||
|| `data` | [`object`](../api-reference/data-types.md) | Event data. For CRM events, the object identifier usually comes in `data[FIELDS][ID]` ||
|| `ts` | [`integer`](../api-reference/data-types.md) | Event sending time in Unix timestamp format ||
|| `auth` | [`object`](../api-reference/data-types.md) | Service authorization data, including `domain`, `member_id`, and `application_token` ||
|#

In a typical scenario, the handler takes the deal identifier from `data[FIELDS][ID]` and then calls the [crm.item.get](../api-reference/crm/universal/crm-item-get.md) method with `entityTypeId = 2` to retrieve full data.

### How to Verify Request Authenticity

Compare the `auth[application_token]` value in the request with the token value shown in the outgoing webhook settings. If the values do not match, the request cannot be considered trusted.

### Outgoing Webhook Limitations

Before configuring an outgoing webhook, consider that:

- in the on-premise version of Bitrix24, an active license is required for an outgoing webhook
- outgoing webhooks are not available in demo modes
- the handler URL must be available from the external network and accept POST requests
- for the on-premise version, you need to open the required [network access](../settings/cloud-and-on-premise/network-access.md)

## When a Webhook Is Not Enough

Choose a [local application](./local-apps.md) or [OAuth 2.0](../settings/oauth/index.md) if:

- the integration must be installed on different Bitrix24 accounts
- you need an interface inside Bitrix24
- you need methods that work only in the application context
- you need centralized authorization management without passing the webhook URL

## Continue Learning

- [{#T}](./developers-area.md)
- [{#T}](./local-apps.md)
- [{#T}](./use-cases.md)
- [{#T}](../first-steps/first-rest-api-call.md)
- [{#T}](../api-reference/scopes/permissions.md)
- [{#T}](../settings/oauth/index.md)
