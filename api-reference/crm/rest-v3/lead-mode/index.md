# CRM Lead Mode in REST 3.0: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note info "" %}

These methods belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

CRM works in one of two modes: the classic mode with leads or the simple mode without leads. The methods of this group check the current mode and switch it. For example, an app that sends website inquiries to CRM can check the mode first and then choose what to create: a lead or a deal right away.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Choose a CRM Mode](https://helpdesk.bitrix24.com/open/24207198/)

## Classic and Simple Modes

#|
|| **Mode** | **How leads work** | **enabled value** ||
|| Classic CRM | Inquiries can be received as leads. A manager works with a lead and then converts it into a deal, contact, or company | `"enabled": true` ||
|| Simple CRM | A new inquiry becomes a deal right away | `"enabled": false` ||
|#

## How to Get Started

1. You can check the current mode with the [crm.lead.isenabled](./crm-lead-is-enabled-rest-v3.md) method
2. If the method returns `"enabled": true`, create leads with the [crm.lead.add](../items/crm-entity-add-rest-v3.md) method. If it returns `false`, create a deal with the [crm.deal.add](../items/crm-entity-add-rest-v3.md) method
3. To change the mode, call [crm.lead.enable](./crm-lead-enable-rest-v3.md) or [crm.lead.disable](./crm-lead-disable-rest-v3.md). The mode changes for all Bitrix24 users

## What Changes When You Switch {#switch}

Switching does not change deals, contacts, or companies. It changes how leads work and the CRM settings.

**Leads.** When leads are disabled:

- leads created before the switch remain in CRM, but the [crm.lead.*](../items/index.md) methods do not work with them: any call, for example, [crm.lead.list](../items/crm-entity-list-rest-v3.md), returns the `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION` error "Access denied" with HTTP status 403. The `crm.lead.isenabled`, `crm.lead.enable`, and `crm.lead.disable` methods work as usual
- Bitrix24 converts each new lead from any source right after it is created, by default into a deal and a contact. Automation rules, the automatic actions at lead stages, do not run for such a lead

**CRM settings.** On any switch, Bitrix24 resets the CRM menu settings for all users. The [crm.lead.enable](./crm-lead-enable-rest-v3.md) method also turns on the "Auto create contacts, companies, deals, invoices and estimates" conversion setting, and [crm.lead.disable](./crm-lead-disable-rest-v3.md) does not turn it off. If the mode has never been switched, the first [crm.lead.enable](./crm-lead-enable-rest-v3.md) call resets the menu and turns on this setting, even when leads are already enabled.

## Permissions and Limitations

The [crm.lead.isenabled](./crm-lead-is-enabled-rest-v3.md), [crm.lead.enable](./crm-lead-enable-rest-v3.md), and [crm.lead.disable](./crm-lead-disable-rest-v3.md) methods can be called only by a CRM administrator. Other users can check the mode with the [previous version method](#previous-version).

If the Bitrix24 plan does not include leads, [crm.lead.isenabled](./crm-lead-is-enabled-rest-v3.md) returns `false`, and [crm.lead.enable](./crm-lead-enable-rest-v3.md) returns the ["Feature Not Available on the Plan"](./crm-lead-enable-rest-v3.md#plan-restriction) error and does not change the mode. The [crm.lead.*](../items/index.md) methods return the same error on such a plan.

## Methods of the Previous Version {#previous-version}

At the `/rest/` address, the mode is returned by the [crm.settings.mode.get](../../crm-settings-mode-get.md) method. Any user can call it. The mode is returned as a number: `1` — classic, `2` — simple. The previous version has no method that switches the mode.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a CRM administrator

#|
|| **Method** | **Description** ||
|| [crm.lead.isenabled](./crm-lead-is-enabled-rest-v3.md) | Checks whether leads are enabled in CRM ||
|| [crm.lead.enable](./crm-lead-enable-rest-v3.md) | Enables leads: CRM switches to the classic mode ||
|| [crm.lead.disable](./crm-lead-disable-rest-v3.md) | Disables leads: CRM switches to the simple mode ||
|#
