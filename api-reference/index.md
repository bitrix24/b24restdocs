# REST API Reference: Bitrix24 tools and methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Using the REST API, an application works with Bitrix24 tools. For example, it can create a deal in the CRM, retrieve a task, upload a file to Drive, process a data change event, or embed a widget into a card.

> Quick links: [all reference sections](#all-methods)

## How to use the reference

Start with what is already known: what data the application should retrieve or change, which method needs to be called, or which permissions to grant.

For example, to work with customers, open [CRM](./crm/index.md). It contains methods for leads, deals, contacts, companies, and other CRM objects. For tasks, the [Tasks](./tasks/index.md) section is suitable; for files, use [Drive](./disk/index.md); for online store orders, use [Online store](./sale/index.md).

If you know the name of a method, look for it in the relevant section or via search. From the names of many methods, you can understand which object the method works with and what action it performs: [crm.deal.add](./crm/deals/crm-deal-add.md) creates a deal, [tasks.task.get](./tasks/tasks-task-get.md) retrieves a task, [disk.file.get](./disk/file/disk-file-get.md) retrieves a file.

If the name of the required method is unknown, open the [reference sections overview](#all-methods) and choose the appropriate section. The section page contains a list of methods and links to related materials.

Check the application permissions in the [Method scopes](./scopes/index.md) section. A scope determines which groups of methods an application can access via the REST API. It is specified when configuring application permissions.

A scope does not grant access to all data by itself. The application will only see the data that the user executing the request has access to.

## What information is on the method pages

Method pages help prepare individual requests. They specify what action the method performs, which parameters to pass, and what response the server will return.

They do not describe a complete workflow. If you need to solve a task entirely, start with the tool's overview page or check the [Tutorials](../tutorials/index.md).

## Reference sections overview {#all-methods}

### REST API basics

#|
|| **Section** | **Description** ||
|| [Data types and parameter formats](./data-types.md) | Basic types, date formats, files, and complex parameters ||
|| [Method scopes](./scopes/index.md) | Application permissions to method groups ||
|| [Common methods and events](./common/index.md) | System methods, application settings, users, and common events ||
|| [Events](./events/index.md) | Registration, deletion, and processing of REST API events ||
|| [REST 3.0](./rest-v3/index.md) | Methods and rules of the new REST API version ||
|| [Deprecated methods](./outdated/index.md) | Methods to support existing integrations ||
|#

### Sales and customers

#|
|| **Section** | **Description** ||
|| [CRM](./crm/index.md) | Leads, deals, contacts, companies, SPAs, and related objects ||
|| [Online Store](./sale/index.md) | Orders, cart, payments, shipments, cash registers, and store events ||
|| [Product Catalog](./catalog/index.md) | Products, offers, prices, warehouses, and warehouse documents ||
|| [Payment Systems](./pay-system/index.md) | Payment processors and payment settings ||
|| [Document Generator](./document-generator/index.md) | Documents, templates, numbering, and roles ||
|| [Signature](./sign/index.md) | Electronic document signing ||
|#

### Automation and company operations

#|
|| **Section** | **Description** ||
|| [Business Processes and Robots](./bizproc/index.md) | Templates, tasks, robots, and business process actions ||
|| [Tasks](./tasks/index.md) | Tasks, checklists, comments, templates, stages, and flows ||
|| [Workgroups and Projects](./sonet-group/index.md) | Workgroups, projects, participants, and scrum ||
|| [Calendar](./calendar/index.md) | Calendars, events, and resources ||
|| [Company Structure](./departments/index.md) | Company departments ||
|| [Time Tracking](./timeman/index.md) | Working time, schedules, and time control ||
|| [Users](./user/index.md) | Users and user fields ||
|#

### Communications

#|
|| **Section** | **Description** ||
|| [Chats](./chats/index.md) | Chats, messages, files, users, and notifications ||
|| [Bot Platform](./chat-bots/index.md) | Chatbots, commands, messages, and bot events ||
|| [Open Channels](./imopenlines/index.md) | Open Channels, sessions, operators, chats, and connectors ||
|| [Telephony](./telephony/index.md) | Telephony, calls, and Voximplant lines ||
|| [Messaging providers, SMS providers](./messageservice/index.md) | Messaging providers and SMS providers ||
|| [News Feed](./log/index.md) | News, comments, and feed events ||
|| [Email Services](./mailservice/index.md) | Email services ||
|#

### Data, files, and content

#|
|| **Section** | **Description** ||
|| [Drive](./disk/index.md) | Files, folders, data store, versions, and permissions ||
|| [How to work with files](./files/index.md) | File transfer formats in REST API ||
|| [Data store](./entity/index.md) | Data store and application data items ||
|| [Universal Lists](./lists/index.md) | Lists, sections, fields, and items ||
|| [Sites and stores](./landing/index.md) | Sites, pages, blocks, templates, and permissions ||
|| [Online booking](./booking/index.md) | Resources, bookings, waiting lists, clients, and events ||
|| [Surveys, polls](./vote/index.md) | Surveys and polls ||
|| [User agreements](./user-consent/index.md) | User agreements ||
|#

### Platform capabilities

#|
|| **Section** | **Description** ||
|| [Widgets](./widgets/index.md) | Application interface embedding points in Bitrix24 ||
|| [BIconnector](./biconnector/index.md) | Sources, connectors, and BI analytics datasets ||
|| [BitrixGPT](./ai/index.md) | Bitrix24 AI capabilities ||
|| [Vibe](./vibe/index.md) | Vibe widgets and interactive items ||
|#
