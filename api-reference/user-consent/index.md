# User Agreements: Overview of Methods

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

{% note info %}

To collect consents for the processing of personal data, use the standard agreement. Add your company details and an email for feedback. For other scenarios, create custom agreements.

{% endnote %}

## Connection with Other Objects

**Users.** Consents can be linked to users through the `USER_ID` parameter. To obtain the user ID, use the [user.get](../user/user-get.md) and [user.search](../user/user-search.md) methods.

{% note tip "User Documentation" %}

- [Сollect customer consent via a CRM form](https://helpdesk.bitrix24.com/open/25821939/)
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