# Managing Lead Cards: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The group of methods `crm.lead.details.configuration.*` manages the settings of the card for two views:

* "General view" — the card view for all employees
* "My view" — personal card settings for the employee

For each card view, sections can be configured, and within each section, a list of fields can be defined. For example, create a section called "Contact Information" and include the fields "Phone" and "Email." For fields that do not pertain to contact information, create a different section.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [CRM item form layout](https://helpdesk.bitrix24.com/open/25791235/)

## Current API Version

Development of the `crm.lead.details.configuration.*` methods has been discontinued. For new development, use the [REST 3.0 card view methods](../../rest-v3/detail-page-layout/index.md). The `crm.lead.details.configuration.*` methods continue to work in existing integrations.

#|
|| **Method with Discontinued Development** | **Replacement** ||
|| `crm.lead.details.configuration.get` | [crm.lead.detailpagelayout.get](../../rest-v3/detail-page-layout/crm-entity-detail-page-layout-get-rest-v3.md) ||
|| `crm.lead.details.configuration.set` | [crm.lead.detailpagelayout.update](../../rest-v3/detail-page-layout/crm-entity-detail-page-layout-update-rest-v3.md) ||
|| `crm.lead.details.configuration.reset` | [crm.item.details.configuration.reset](../../universal/item-details-configuration/crm-item-details-configuration-reset.md) with `entityTypeId: 1`; REST 3.0 has no equivalent ||
|| `crm.lead.details.configuration.forceCommonScopeForAll` | [crm.lead.detailpagelayout.setcommonforall](../../rest-v3/detail-page-layout/crm-entity-detail-page-layout-set-common-for-all-rest-v3.md) ||
|#

In REST 3.0, repeat leads have their own views: `personal_returning_customer` and `common_returning_customer`.

## How to Configure the Card

1. Retrieve the identifiers of the fields you want to display in the card with the [crm.lead.fields](../crm-lead-fields.md) method: `TITLE`, `STATUS_ID`, `PHONE`, and others. Besides lead fields, the card contains composite elements such as `CLIENT` and `OPPORTUNITY_WITH_CURRENCY` — their names are returned by [crm.lead.details.configuration.get](./crm-lead-details-configuration-get.md).
2. Review the current structure of the card with the [crm.lead.details.configuration.get](./crm-lead-details-configuration-get.md) method — it returns a list of sections with nested fields, or `null` if no configuration has been saved.
3. Pass the new structure with the [crm.lead.details.configuration.set](./crm-lead-details-configuration-set.md) method. The new structure replaces the entire saved configuration of the selected view.
4. If the result does not suit you, return to the default configurations with the [crm.lead.details.configuration.reset](./crm-lead-details-configuration-reset.md) method.
5. To force the general view of the card onto all employees and delete their personal configurations, call [crm.lead.details.configuration.forceCommonScopeForAll](./crm-lead-details-configuration-force-common-scope-for-all.md).

## Card Configuration Structure

A configuration is an array of sections with nested fields in `elements`. An example of the `result` field in the [crm.lead.details.configuration.get](./crm-lead-details-configuration-get.md) response with one section and two lead fields:

```json
{
    "result": [
        {
            "name": "lead_info",
            "title": "About the Lead",
            "type": "section",
            "elements": [
                {"name": "TITLE"},
                {"name": "STATUS_ID"}
            ]
        }
    ]
}
```

#|
|| **Level** | **Fields** | **Description** ||
|| Section | `name`, `title`, `type`, `elements` | `name` — section name, `title` — heading, `type` — the `section` type, `elements` — an array of lead fields ||
|| Section field | `name` | A lead field identifier from `crm.lead.fields` or a card element name from the `crm.lead.details.configuration.get` response ||
|#

The [crm.lead.details.configuration.set](./crm-lead-details-configuration-set.md) method accepts the array of sections from `result` in the `data` parameter.

## Configuration Scope

The `scope` parameter selects the card view, and `userId` selects the employee. By default, `scope` is `P`, so the methods work with the personal configurations of the calling user.

#|
|| **Card View** | **What to Pass** | **Who Can Read** | **Who Can Change and Reset** ||
|| General view — the card for all employees | `scope = C` | Any user | A user with the "Edit CRM preferences" or "Manage CRM item form layout" permission ||
|| Own personal view | `scope = P` without `userId` | Any user | Any user ||
|| Another employee's personal view | `scope = P` and that employee's `userId` | CRM administrator | CRM administrator ||
|#

A CRM administrator is a user with the "Edit CRM preferences" permission. Both permissions are granted for the entire CRM and cannot be granted for leads separately. The `forceCommonScopeForAll` method is available to a user with either of these permissions.

## Linking Lead Cards with Other Objects

**User.** The user identifier `userId` is used when setting personal card settings. The user identifier can be obtained using the method [user.get](../../../user/user-get.md).

**Lead Fields.** The field identifiers are used when setting visible fields in the card section. The identifiers for system and custom lead fields can be obtained using the method [crm.lead.fields](../crm-lead-fields.md).

## Configuring Cards for Simple and Repeat Leads

Leads can be of two types: simple and repeat. A lead becomes repeat if the "Client" field is filled. Repeat leads do not have contact fields such as "Phone," "Email," or "Address" — this information is stored in the contact or company linked through the "Client" field.

Configurations for simple and repeat lead cards are retained separately. The lead type is selected by the `leadCustomerType` parameter inside the `extras` object:

#|
|| **Value** | **Which Lead Card Is Configured** ||
|| `1` | Simple lead ||
|| `2` | Repeat lead ||
|#

The `extras` parameter is optional. If `extras` or the `leadCustomerType` key in it is not passed, the methods work with the simple lead card.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#|
|| **Method** | **Description** ||
|| [crm.lead.details.configuration.set](./crm-lead-details-configuration-set.md) | Sets lead card settings ||
|| [crm.lead.details.configuration.get](./crm-lead-details-configuration-get.md) | Retrieves lead card settings ||
|| [crm.lead.details.configuration.reset](./crm-lead-details-configuration-reset.md) | Resets lead card settings ||
|| [crm.lead.details.configuration.forceCommonScopeForAll](./crm-lead-details-configuration-force-common-scope-for-all.md) | Forces a common lead card for all users ||
|#