# How to Access the REST API

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

You can access the Bitrix24 REST API in three ways:

- choose a Vibe+ plan for ongoing work with the REST API
- enable the 15-day trial to test the REST API before purchasing
- request an NFR key if you are developing mass-market applications for the Bitrix24 Market

## Permanent Access

For ongoing REST API access in Bitrix24 Cloud, choose a paid plan in the Vibe+ lineup. The Essentials lineup does not include REST API access. If you already have a paid subscription, your existing REST API access may continue during the transition period; the end date depends on your subscription terms.

1. Check your current plan in the My plan widget. [Compare the available plans and prices](https://www.bitrix24.com/prices/), then choose a Vibe+ plan that suits your needs.

   ![My plan section](_images/plan.png)

2. Purchase or switch to the selected Vibe+ plan. The available options depend on your current subscription.

{% note tip "" %}

- [My Plan Widget Features](https://helpdesk.bitrix24.com/open/21293016/)
- [Vibe+ plans: what changes for paid customers](https://helpdesk.bitrix24.com/open/26027119/)

{% endnote %}

## Trial Access

You can activate a free 15-day trial of the Professional Vibe+ plan. It includes REST API access for testing and development. The trial can be activated only once; it does not pause an existing commercial subscription.

To activate the trial:

1. Open the My plan widget in the top right. If you are on the Free plan, click Buy Now.
2. In the 15-day trial section, click Activate.
3. In the slider panel, click Activate Trial Mode.

{% note warning "" %}

In the Bitrix24 Self-Hosted version, you can only activate the Market trial period if you have an active subscription. If you are using a trial key for the Self-Hosted version, the Market demo mode will be unavailable.

{% endnote %}

{% note tip "" %}

- [Free 15-Day Trial](https://helpdesk.bitrix24.com/open/20237014/)

{% endnote %}

## Access for Technology Partners

If you are developing mass-market applications to be listed in the Bitrix24 Market, request a special NFR key for a test Bitrix24 environment with REST API access.

To obtain an NFR key:

1. Register as a technology partner. Fill out the application form on the [Developer's area website](https://vendors.bitrix24.com/technology-partnership/) and click the Become a Partner button.
2. After gaining access to the Developer's area, submit an NFR key request via the internal Helpdesk chat.

{% note tip "" %}

- [Mass-Market Applications Overview](../market/index.md)
- [Technology Partnership](../market/technology-partnership.md)

{% endnote %}

## Factors Affecting Request Execution

After activating access, the request result depends on user permissions, `scope`, and Bitrix24 network settings.

### User Permissions

The REST API executes requests on behalf of the user who sends them. The API does not extend access permissions: through the API, you can only perform actions that the user can perform within the interface. For example, if a user cannot see a task in the task list, they will not be able to retrieve it via a REST API method.

To obtain maximum access, use an `administrator` account.

### Restrictions via Scopes

Permissions to execute REST API methods are additionally regulated via `scope`. Bitrix24 scopes determine which methods an application or webhook can access. If a user has access to the data, but the application or webhook does not have the necessary `scope`, the method will not execute.

{% note tip "" %}

- [Available Bitrix24 Scopes](../api-reference/scopes/permissions.md)

{% endnote %}

### Network Access for Self-Hosted Versions

In the self-hosted version of Bitrix24, the necessary network connections must be open. If external access is restricted, the REST API will be unavailable.

{% note tip "" %}

- [Required Network Access](../settings/cloud-and-on-premise/network-access.md)

{% endnote %}

## What's Next

Once REST API access is active on your plan or during the trial, you can proceed to making your first request to the REST API.

{% note tip "" %}

- [How to Make Your First API Request](./first-rest-api-call.md)

{% endnote %}
