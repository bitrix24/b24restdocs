# Authorization in REST

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Bitrix24 REST API methods are called with HTTP requests to the address of a specific Bitrix24 account, from any software that supports the HTTP protocol. Each request is executed on behalf of a user and with that user's permissions: if an employee cannot see a deal in CRM, the method does not return it through the API either. Access is further restricted by scopes, the authorization method, and the conditions of the specific method.

Therefore, in addition to the method parameters, each request passes authorization data. Without it, Bitrix24 rejects the request with the `NO_AUTH_FOUND` error.

Authorization data is passed in one of two ways:

- [Inbound Webhook](#webhook) — a permanent secret code in the request URL. Suitable for integrations with a single Bitrix24 account
- [OAuth 2.0 Token](#oauth) — a temporary token in the `auth` parameter. Local and mass-market applications receive it

To choose the right method for your task, see the [How to Call REST API Methods](./index.md#auth) section overview.

## Inbound Webhooks {#webhook}

Example of accessing the REST API through an inbound webhook:

```bash
curl -X POST \
-H "Content-Type: application/json" \
-d '{
	"entityTypeId": 2,
	"fields": {
		"title": "New Deal",
		"typeId": "SALE",
		"stageId": "NEW"
	}
}' \
https://your-domain.bitrix24.com/rest/1/8g9l071eismy9q2l/crm.item.add.json
```

The request URL contains:

- the Bitrix24 address — `your-domain.bitrix24.com`
- the ID of the user who created the webhook — `1`
- the webhook secret code — `8g9l071eismy9q2l`
- the [crm.item.add](../../api-reference/crm/universal/crm-item-add.md) method, which adds a CRM item. The value `entityTypeId: 2` stands for a deal

The method parameters, `entityTypeId` and `fields` in this example, are passed in the body of the POST request.

The user ID and the secret code in the URL serve as authorization. The method is executed with the permissions of the user who created the webhook, and only within the [Scopes](../../api-reference/scopes/permissions.md) selected in the webhook settings. The webhook code grants access to Bitrix24 data, so treat it like a password: do not publish it or pass it to code that runs in the browser.

Webhooks are suitable for:

- one-time data import or export
- simple integrations with company systems: ERP, time tracking, hardware and software monitoring
- automating lead and deal processing in CRM automation rules and triggers

A webhook is easier to implement because it does not require the OAuth 2.0 protocol. By default, any employee can create a webhook, and an administrator can restrict this permission. Some methods are not available to a webhook because they require an application context. For example, the [placement.bind](../../api-reference/widgets/placement-bind.md) method returns the `WRONG_AUTH_TYPE` error when called through a webhook.

To learn how to create a webhook, set up employee access, and test a method, see the [Inbound and Outbound Webhooks](../../local-integrations/local-webhooks.md) page.

## Applications with OAuth 2.0 Authorization {#oauth}

Example of accessing the REST API with a temporary authorization token:

```bash
curl -X POST \
-H "Content-Type: application/json" \
-d '{
	"entityTypeId": 2,
	"fields": {
		"title": "New Deal",
		"typeId": "SALE",
		"stageId": "NEW"
	},
	"auth": "807ca26600631fce00007a4b00000001f0f107255033363e91ab16442bd901b2571ed9"
}' \
https://your-domain.bitrix24.com/rest/crm.item.add.json
```

The request contains:

- the Bitrix24 address — `your-domain.bitrix24.com`
- the [crm.item.add](../../api-reference/crm/universal/crm-item-add.md) method, which adds a deal
- the authorization token in the `auth` parameter

The `access_token` that the application passes in the `auth` parameter serves as authorization. Bitrix24 uses it to identify the application and the user on whose behalf the request is executed. The method is executed with this user's permissions and within the application's scopes: the example requires the `crm` scope and permission to add deals.

A token is valid for a limited time, after which it must be refreshed. How applications receive and renew tokens is described in the [OAuth 2.0](../oauth/index.md) section. Like a webhook code, tokens grant access to Bitrix24 data: do not publish them or write them to logs, and keep them in secure storage on the application server.

Applications can be local or mass-market.

**Local applications** are installed on a single Bitrix24 account, without publication in the catalog. Unlike webhooks, they are suitable for tasks that require a custom interface:

- reports
- handlers for specific business logic
- solutions that manage user access
- chatbots and applications that extend the messenger's capabilities
- additional workflow actions

A local application can also call methods that require an application context. By default, only an administrator can add a local application, and they can grant this permission to other employees.

**Mass-market applications** are published in the [Bitrix24 Marketplace](../../market/index.md) catalog. They can be installed on multiple Bitrix24 accounts, and you can charge for the app's features. Publication is governed by the Marketplace rules, so review them before you start development.

## If Authorization Fails {#errors}

#|
|| **Status** | **Code** | **When it occurs** | **What to do** ||
|| `401` | `NO_AUTH_FOUND` | The request contains no authorization data | Pass the webhook code in the request URL or the token in the `auth` parameter ||
|| `401` | `INVALID_CREDENTIALS` | There is no active webhook with this user ID and code | Make sure the webhook exists and is active, and copy its URL from the settings again ||
|| `401` | `invalid_token` | Bitrix24 did not accept the token, for example, because it was copied incorrectly | Request a new token using the [OAuth 2.0 Protocol](../oauth/index.md) ||
|| `401` | `expired_token` | The token has expired | Refresh the token as described in the article [Automatic Renewal of OAuth 2.0 Tokens](../oauth/auto-renewal.md) ||
|| `403` | `WRONG_AUTH_TYPE` | The method requires an application context, but the request came through a webhook | Call the method from an application ||
|#

System errors and how to handle them are described in the [Error Codes](../../error-codes.md) article, and errors of a specific method are described on its page.

## Continue Learning

- [{#T}](./index.md)
- [{#T}](../../first-steps/first-rest-api-call.md)
- [{#T}](../../local-integrations/local-webhooks.md)
- [{#T}](../oauth/index.md)
