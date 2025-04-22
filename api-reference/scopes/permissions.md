# Access Permissions for Methods

Access permissions for executing specific REST API methods are regulated through a mechanism called scopes (SCOPE). When you add a mass-market solution in the Partner's area or an on-premise solution to your specific Bitrix24, you specify the list of necessary Bitrix24 scopes required for the application to function.

The binding to a specific scope is indicated in the description of each REST method at the very beginning. For example,

> Scope: `CRM`
>
> Who can execute the method: any user

Also, pay attention to the note "Who can execute the method." Some methods can only be called on behalf of a user with administrative rights in a specific Bitrix24.

Let's consider a specific situation where your solution integrates Bitrix24 with external telephony, and you are using the methods `telephony.externalcall.register` and `telephony.externalcall.finish`, which also add leads to the CRM, but do not explicitly call CRM methods like `crm.lead.add` and `crm.activity.add`. In this case, your application will require the telephony scope, but not the crm scope.

## Current Bitrix24 Scopes

#|
|| **Scope Code** | **Scope Name**| **Bitrix24 Tool**||
|| **ai_admin** | [Channel for registering a custom service for processing requests](../ai/index.md)| Copilot ||
|| **bizproc** | [Business Processes](../bizproc/index.md) | Business Processes, RPA, CRM Automation Rules ||
|| **booking** | [Online Booking](../booking/index.md) | Online Booking ||
|| **calendar** | [Calendar](../calendar/index.md) | Calendar ||
|| **call** | Telephony (making calls). The scope includes methods: [voximplant.infocall.startwithsound](../telephony/voximplant/voximplant-infocall-start-with-sound.md), [voximplant.infocall.startwithtext](../telephony/voximplant/voximplant-infocall-start-with-text.md)| Telephony ||
|| **cashbox** | [Cash Registers](../sale/cashbox/index.md) | Cash Registers ||
|| **catalog** | [Trade Catalog](../catalog/index.md) | Trade Catalog, inventory management ||
|| **crm** | [CRM](../crm/index.md) | CRM ||
|| **documentgenerator, crm.documentgenerator** | [Document Generator](../document-generator/index.md), [CRM Document Generator](../crm/document-generator/index.md) | Document Generator ||
|| **delivery** | [Deliveries](../sale/delivery/index.md) | Online Store, CRM ||
|| **department** | [Company Structure](../departments/index.md) | Company Structure ||
|| **disk** | [Drive](../disk/index.md) | Bitrix24.Drive ||
|| **entity** | [Data Storage](../entity/index.md) | Data Storage ||
|| **im** | [Chat and Notifications](../chats/index.md) | Chat and Notifications ||
|| **imbot** | [Creating and Managing Chat Bots](../chat-bots/index.md) | Chat Bots ||
|| **imopenlines** | [Open Channels](../imopenlines/index.md) | Open Channels ||
|| **landing** | [Sites](../landing/index.md) | Sites ||
|| **lists** | [Lists](../lists/index.md) | Universal Lists ||
|| **log** | [Live Feed](../log/index.md) | News Feed ||
|| **mailservice** | [Mail Services](../mailservice/index.md) | Mail Services ||
|| **messageservice** | [Message Service](../messageservice/index.md) | Message Service ||
|| **pay_system** | [Payment Systems](../pay-system/index.md) | Payment Systems ||
|| **pull** | [Pull&Push](../interactivity/push-and-pull/index.md) | Pull&Push ||
|| **rpa** | [Business Automation](../outdated/rpa/index.md) | Business Automation ||
|| **sale** | [Online Store](../sale/index.md) | Online Store ||
|| **sign.b2e** | [Signature](../sign/index.md) | e-Signature for HR + Government Key ||
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

[*key_task]: In addition, there are three deprecated scopes available — tasks, tasks_extended, tasksmobile. They should not be used.