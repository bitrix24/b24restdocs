# Available Scopes in Bitrix24

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A scope is a code for a group of methods. It defines which Bitrix24 tools an application or a webhook can access through the REST API. To find out how to select scopes for an application scenario and how a scope differs from user permissions, see [Access Permissions for Methods and Scopes](./index.md).

> Quick navigation: [Scope Codes](#codes)

## Where to Specify a Scope

For an application, scopes are selected when it is added, and the place depends on the solution type.

- A mass-market solution is added in the Developer's Area. Scopes are specified in the technical specifications as the system sections the application interacts with. The procedure is described in the article [How to Add a Solution in the Developer's Area](../../market/preparing-to-publish/how-to-add-app.md).
- A local application is added in Bitrix24, in *Applications > Developer resources*. The procedure is described in the article [Local Applications](../../local-integrations/local-apps.md).

For a webhook, scopes are selected when it is created, at the *Specify access permissions* step. Requests run within the selected scopes and with the permissions of the employee who created the webhook. For details, see [Incoming and Outgoing Webhooks](../../local-integrations/local-webhooks.md).

## What Happens if a Method Is Called Outside the Granted Scope

If an application or a webhook calls a method whose scope has not been granted, Bitrix24 returns the `insufficient_scope` error and does not execute the request. Response to a call from an application:

```json
{
    "error": "insufficient_scope",
    "error_description": "The request requires higher privileges than provided by the access token"
}
```

For a webhook, only `error_description` differs — it contains `provided by the webhook token`.

To make the call succeed, add the required scope to the application or webhook settings. Other system errors are collected in the article [Error Codes](../../error-codes.md).

## When a Scope Alone Is Not Enough

Some methods work only in the application context. When a webhook calls such a method, Bitrix24 returns the `WRONG_AUTH_TYPE` error with the `Application context required` description. The [placement.bind](../widgets/placement-bind.md) method behaves this way, for example.

## Scope Codes {#codes}

#|
|| **Scope Code** | **Scope Name**| **Bitrix24 Tool**||
|| **ai_admin** | [Channel for registering a user service to process requests](../ai/index.md)| Copilot ||
|| **biconnector** | [BI Analytics Connector](../biconnector/index.md) | BIconnector ||
|| **bizproc** | [Business Processes](../bizproc/index.md) | Business processes, RPA, CRM robots ||
|| **booking** | [Online Booking](../booking/index.md) | Online Booking ||
|| **calendar** | [Calendar](../calendar/index.md) | Calendar ||
|| **call** | Telephony (making calls). The scope includes methods: [voximplant.infocall.startwithsound](../telephony/voximplant/voximplant-infocall-start-with-sound.md), [voximplant.infocall.startwithtext](../telephony/voximplant/voximplant-infocall-start-with-text.md)| Telephony ||
|| **cashbox** | [Cash Registers](../sale/cashbox/index.md) | Cash Registers ||
|| **catalog** | [Product Catalog](../catalog/index.md) | Product catalog, inventory management ||
|| **contact_center** | [Contact Center Widget](../widgets/contact-center.md) | Contact Center ||
|| **crm** | [CRM](../crm/index.md) | CRM ||
|| **documentgenerator** | [Document Generator](../document-generator/index.md), [CRM Document Generator](../crm/document-generator/index.md) | Document Generator ||
|| **delivery** | [Delivery](../sale/delivery/index.md) | Online store, CRM ||
|| **department** | [Company Structure](../departments/index.md) | Company Structure ||
|| **disk** | [Drive](../disk/index.md) | Bitrix24.Drive ||
|| **entity** | [Data store](../entity/index.md) | Data store ||
|| **humanresources** | [Company Structure REST 3.0](../departments/index.md) | Company Structure ||
|| **humanresources.hcmlink** | [e-Signature Integration with HR Systems](../sign/hcm-link/index.md) | e-Signature ||
|| **im** | [Chat and Notifications](../chats/index.md) | Chat and Notifications ||
|| **imbot** | [Creating and managing Chatbots](../chat-bots/index.md) | Chat bots ||
|| **imconnector** | [Connectors for external messengers](../imopenlines/imconnector/index.md) | Open Channels ||
|| **imopenlines** | [Open Channels](../imopenlines/index.md) | Open Channels ||
|| **intranet** | [Widgets](../widgets/index.md) | Widgets ||
|| **landing** | [Websites](../landing/index.md) | Websites ||
|| **lists** | [Lists](../lists/index.md) | Universal lists ||
|| **log** | [Live Feed](../log/index.md) | News feed ||
|| **mailservice** | [Email Services](../mailservice/index.md) | Email services ||
|| **main** | [Event Log](../event-log/index.md) | Event Log ||
|| **messageservice** | [Messaging Service](../messageservice/index.md) | Messaging Service ||
|| **mobile** | [Mobile App](../widgets/mobile-app.md) | Mobile App ||
|| **pay_system** | [Payment Systems](../pay-system/index.md) | Payment Systems ||
|| **placement** | [Widgets](../widgets/index.md) | App Embedding ||
|| **pull** | [Pull&Push](../../settings/interactivity/push-and-pull/index.md) | Pull&Push ||
|| **rpa** | [Business Automation](../outdated/rpa/index.md) | Business Automation ||
|| **sale** | [Online store](../sale/index.md) | Online store ||
|| **salescenter** | [CRM. Payment](../crm/universal/payment/index.md) | Chat sales ||
|| **sign.b2e** | [e-Signature](../sign/index.md) | e-Signature ||
|| **[sonet_group](*key_sonet)** | [Social Network Working Groups](../sonet-group/sonet-group-create.md) | Social Network Working Groups ||
|| **[task](*key_task)** | [Tasks](../tasks/index.md) | Tasks ||
|| **telephony** | [Telephony](../telephony/index.md) | Telephony ||
|| **timeman** | [Time Tracking](../timeman/index.md) | Time Tracking ||
|| **user** | [Users](../user/index.md)
Versions:
- **user_brief** — Users (minimal)
- **user_basic** — Users (basic) | Users ||
|| **user.userfield** | [User custom fields](../user/userfields/index.md) | Custom fields ||
|| **userfieldconfig** | [Custom field settings](../crm/universal/userfieldconfig/index.md) | Custom field settings ||
|| **userconsent** | [Working with agreements](../user-consent/index.md) | Working with agreements ||
|| **vote** | [Surveys](../vote/index.md) | Working with surveys, voting ||
|#

[*key_task]: Additionally, three deprecated scopes are available — tasks, tasks_extended, tasksmobile. They should not be used.

[*key_sonet]: The socialnetwork scope does not grant access to any method. To work with working groups, specify sonet_group.
