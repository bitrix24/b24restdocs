# Method Permissions

Permissions for executing various REST API methods are regulated through a scope mechanism (SCOPE). When you add a mass-market solution in the partner's account or a local solution in your specific Bitrix24, you specify the list of necessary Bitrix24 scopes for the operation of the particular application.

The binding to a specific scope is indicated in the description of each REST method at the very beginning. For example,

> Scope: `CRM`
>
> Who can execute the method: any user

Also, pay attention to the note "Who can execute the method." Some methods can only be called on behalf of a user with administrative rights in a specific Bitrix24.

Let's consider a specific situation where your solution integrates Bitrix24 with external telephony, and you are using the methods `telephony.externalcall.register` and `telephony.externalcall.finish`, which also add leads to the CRM, but do not explicitly call CRM methods like `crm.lead.add` and `crm.activity.add`. In this case, your application will require the telephony scope, but not the crm scope.

## Current Bitrix24 Scopes

#|
|| **Scope Code** | **Scope Name**| **Bitrix24 Tool**||
|| **ai_admin** | [Channel for registering a user service for processing requests](../ai/index.md)| Copilot ||
|| **bizproc** | [Business Processes](../bizproc/index.md) | Business processes, RPA, CRM automation rules ||
|| **calendar** | [Calendar](../calendar/index.md) | Calendar ||
|| **call** | Telephony (making calls). The scope includes methods: [voximplant.infocall.startwithsound](../telephony/voximplant/voximplant-infocall-start-with-sound.md), [voximplant.infocall.startwithtext](../telephony/voximplant/voximplant-infocall-start-with-text.md)| Telephony ||
|| **cashbox** | [Cash Registers](../sale/cashbox/index.md) | Cash Registers ||
|| **catalog** | [Trade Catalog](../catalog/index.md) | Trade catalog, inventory management ||
|| **crm** | [CRM](../crm/index.md) | CRM ||
|| **documentgenerator, crm.documentgenerator** | [Document Generator](../document-generator/index.md), [CRM Document Generator](../crm/document-generator/index.md) | Document Generator ||
|| **delivery** | [Deliveries](../sale/delivery/index.md) | Online store, CRM ||
|| **department** | [Company Structure](../departments/index.md) | Company Structure ||
|| **disk** | [Disk](../disk/index.md) | Bitrix24.Disk ||
|| **entity** | [Data Storage](../entity/index.md) | Data Storage ||
|| **im** | [Chat and Notifications](../chats/index.md) | Chat and Notifications ||
|| **imbot** | [Creating and Managing Chatbots](../chat-bots/index.md) | Chatbots ||
|| **imopenlines** | [Open Lines](../imopenlines/index.md) | Open Lines ||
|| **landing** | [Websites](../landing/index.md) | Websites ||
|| **lists** | [Lists](../lists/index.md) | Universal Lists ||
|| **log** | [Live Feed](../log/index.md) | News Feed ||
|| **mailservice** | [Mail Services](../mailservice/index.md) | Mail Services ||
|| **messageservice** | [Message Service](../messageservice/index.md) | Message Service ||
|| **pay_system** | [Payment Systems](../pay-system/index.md) | Payment Systems ||
|| **pull** | [Pull&Push](../interactivity/push-and-pull/index.md) | Pull&Push ||
|| **rpa** | [Business Automation](../outdated/rpa/index.md) | Business Automation ||
|| **sale** | [Online Store](../sale/index.md) | Online Store ||
|| **sonet_group, socialnetwork** | [Social Network Workgroups](../sonet-group/sonet-group-create.md) | Social Network Workgroups ||
|| **task** | [Tasks](../tasks/index.md) | Tasks ||
|| **telephony** | [Telephony](../telephony/index.md) | Telephony ||
|| **timeman** | [Time Tracking](../timeman/index.md) | Time Tracking ||
|| **user** | [Users](../user/index.md) 
Versions: 
- **user_brief** — Users (minimal) 
- **user_basic** — Users (basic) | Users ||
|| **user.userfield** | [User Custom Fields](../user/userfields/index.md) | User Custom Fields ||
|| **userfieldconfig** | [User Field Settings](../crm/universal/userfieldconfig/index.md) | User Field Settings ||
|| **userconsent** | [Working with Agreements](../user-consent/index.md) | Working with Agreements ||
|#

[*key_task]: In addition, three deprecated scopes are available — tasks, tasks_extended, tasksmobile. They should not be used.