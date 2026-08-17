# User Agreements: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In Bitrix24, you can create user agreements. They are necessary if you process personal data, send newsletters, or perform other actions that require user consent.

With the REST API, you can:

- retrieve lists of agreements
- upload agreement texts
- save obtained consents

Agreements should be created and modified through the Bitrix24 interface.

> Quick navigation: [all methods](#all-methods)

## Types of Agreements

There are two types of agreements in Bitrix24: standard and custom.

- Standard agreements are created from pre-installed templates. They support localization and variable replacement through the `replace` parameter in the [userconsent.agreement.text](./user-consent-agreement-text.md) method.

- Custom agreements are created manually. They contain arbitrary HTML text and do not support variable substitution.

{% note info "" %}

To collect consents for processing personal data, use the standard agreement. Add your company details and an email for feedback. For other scenarios, create custom agreements.

{% endnote %}

## How to Obtain Consent

1. Retrieve the list of agreements using the [userconsent.agreement.list](./user-consent-agreement-list.md) method and find the agreement you need by its name `NAME`. Check the activity flag `ACTIVE`: for a disabled agreement, it equals `N`.
2. Retrieve the text of the agreement using the [userconsent.agreement.text](./user-consent-agreement-text.md) method. Provide the agreement identifier `id`. For a standard agreement, provide your company details and the confirmation button text in the `replace` parameter.
3. Show the user the text `TEXT` and the button label `LABEL` from the response. Through the REST API, Bitrix24 returns only the text of the agreement — you display the consent form on your side.
4. Save the obtained consent using the [userconsent.consent.add](./user-consent-consent-add.md) method. Provide the agreement identifier `AGREEMENT_ID` and the user's IP address `IP`. The method returns the identifier of the saved consent.

## Relationships with Other Objects

**Users.** Consents can be linked to users through the `USER_ID` parameter. To obtain the user ID, use the [user.get](../user/user-get.md) and [user.search](../user/user-search.md) methods.

{% note tip "User Documentation" %}

- [Collect customer consent via a CRM form](https://helpdesk.bitrix24.com/open/25821939/)
- [Obtain consent for personal data processing in Open Channels](https://helpdesk.bitrix24.com/open/25828127/)
- [Consent to receive newsletters](https://helpdesk.bitrix24.com/open/23269576/)
- [Personal data processing](https://helpdesk.bitrix24.com/open/14190482/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`userconsent`](../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [userconsent.agreement.list](./user-consent-agreement-list.md) | Retrieves a list of agreements ||
|| [userconsent.agreement.text](./user-consent-agreement-text.md) | Retrieves the text of an agreement ||
|| [userconsent.consent.add](./user-consent-consent-add.md) | Saves the obtained user consent ||
|#