# REST API Reference: Bitrix24 tools and methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Using the REST API, an application works with Bitrix24 tools. For example, it can create a deal in the CRM, retrieve a task, upload a file to Drive, process a data change event, or embed a widget into a CRM card.

> Quick links: [all reference sections](#all-methods)

## How to find the right method

A method name hints at the object and the action: [crm.deal.add](./crm/deals/crm-deal-add.md) creates a deal, [tasks.task.get](./tasks/tasks-task-get.md) retrieves a task, [disk.file.get](./disk/file/disk-file-get.md) retrieves a file.

A method can be checked programmatically, without opening its page. The [method.get](./common/system/method-get.md) method shows whether a method exists and whether it is available to the application. The [scope](./common/system/scope.md) method returns the list of granted scopes. A complete machine-readable list of the new version's methods is available in the [OpenAPI documentation](./rest-v3.md#openapi).

## How to work with the REST API

If you have not sent requests to the REST API yet, start with [How to Make Your First API Request](../first-steps/first-rest-api-call.md). The steps below describe working with the old version of the REST API. The differences of the new version are collected in the [Which API version to choose](#version) block.

1. Determine the object and find the method in the [reference sections overview](#all-methods).

2. Grant permissions. Almost every method belongs to a [scope](./scopes/index.md) — a group of methods that the application gets access to. Scopes are listed when configuring application permissions. System methods work without a separate scope: the Scope block on their pages contains `basic`. A scope does not grant access by itself — the application works only with the data that is visible to the user executing the request.

3. Choose an authorization method. A webhook is suitable for an integration inside your own Bitrix24, and an application with OAuth 2.0 authorization is suitable for solutions installed on different Bitrix24 accounts. Both methods are described in [Authorization in REST](../settings/how-to-call-rest-api/authorization.md), and the tools for local integrations are described in [Tools for Local Integrations](../local-integrations/index.md).

4. Call the method. The address depends on the authorization method:

    - webhook — the user identifier and the secret code are passed in the path: `https://your-domain.bitrix24.com/rest/USER_ID/WEBHOOK_CODE/method`
    - application — the token is passed in the `auth` parameter: `https://your-domain.bitrix24.com/rest/method?auth=ACCESS_TOKEN`

    The rules for passing parameters are described in [How a Request Is Executed](../settings/how-to-call-rest-api/general-principles.md), and value formats are described in [Data types and parameter formats](./data-types.md). Field names and the format of boolean values depend on the module and the API version, so check them on the method page.

5. Parse the response. The method data is returned in the `result` field, and the execution time is returned in the `time` field. List methods return no more than 50 items at a time and pass the offset for the next page in the `next` field. Details are in [How to Call REST API Methods](../settings/how-to-call-rest-api/index.md) and [Features of List Methods](../settings/how-to-call-rest-api/list-methods-pecularities.md).

6. Handle errors. Instead of data, the server returns the `error` and `error_description` fields. The general list is in [Error Codes](../error-codes.md), and the errors of a specific method are listed on its page.

After the first working call, the following will come in handy:

- [Events](./events/index.md) — handler registration, if the integration has to react to changes in Bitrix24 instead of only requesting data
- [batch](../settings/how-to-call-rest-api/batch.md) — combines up to 50 calls into a single request
- [REST API Limits](../limits.md) — restrictions on the intensity and resource consumption of requests
- [SDK for Bitrix24 Development](../sdk/index.md) — ready-made clients for JavaScript, PHP, Python, and Go

## Which API version to choose {#version}

Both REST API versions work simultaneously and do not replace each other. Some sections have been migrated to [REST 3.0](./rest-v3.md). Such sections are marked in the tables below, and the remaining methods are called as before. The same method can exist in both versions with different parameters and a different response. If a method is available in both versions, REST 3.0 is recommended for new development: a unified response format, related objects in a single request, and protection against repeated execution using the `Idempotency-Key` header.

In REST 3.0, the call and the response parsing differ:

- the address contains the `/rest/api/` segment
- the request body is passed in JSON format only
- a list arrives as an array in the `items` key inside `result`
- the page size and offset are set by the `pagination` object in the request — the composition of its fields is described on the method page
- an error arrives as an `error` object with the `code`, `message`, and `validation` fields
- the body of a [batch](./rest-v3.md#batch) packet is passed as a JSON array of objects with the `method` and `query` fields
- the packet format with the `cmd` wrapper does not work in the new version

The banner at the beginning of a method page shows its version. The same place carries a warning if the development of a method family has stopped — this also happens outside the deprecated methods section. The full version comparison is in the [Comparison of REST 3.0 with the old version of the API](./rest-v3.md#table) table.

## When to open a tutorial

Method pages help prepare individual requests, but they do not describe a complete workflow. If a task needs to be solved from start to finish, start with the tool's overview page or open the [Tutorials](../tutorials/index.md) — they cover ready-made scenarios made up of several calls.

## Reference sections overview {#all-methods}

Sections are grouped by task. The list of a section's methods is in the block with the `#all-methods` anchor at the end of its page. In large sections, the same block also lists subsections and their methods. Three sections are arranged differently:

- Bot Platform — the methods are split across subsections, and the entry point is the section index page
- Widgets — the `placement.*` methods are located in the section root, and the catalog of embedding points is on the [Placement Catalog](./widgets/placements.md) page
- Deprecated methods — the methods are moved to the RPA subsection

Data types and parameter formats, Method scopes, and How to work with files contain no methods of their own — these are rules common to the entire API. The [REST 3.0](./rest-v3.md) section describes the rules of the new version and its two own methods: `rest.documentation.openapi` and `rest.scope.list`.

### REST API basics

#|
|| **Section** | **Description** ||
|| [Data types and parameter formats](./data-types.md) | Basic types, date formats, files, and complex parameters ||
|| [Method scopes](./scopes/index.md) | Application permissions to method groups ||
|| [Common methods and events](./common/index.md) | System methods, application settings, users, and common events ||
|| [Events](./events/index.md) | Handler registration, offline queue, and deletion of REST API events ||
|| [REST 3.0](./rest-v3.md) | Call rules and response format of the new REST API version, and the list of sections migrated to it ||
|| [Deprecated methods](./outdated/index.md) | Robotic process automation (RPA) methods whose development has stopped — not for new solutions ||
|#

### Sales and customers

#|
|| **Section** | **Description** ||
|| [CRM](./crm/index.md) | Leads, deals, contacts, companies, SPAs, quotes, requisites, timeline activities, stages and pipelines, and CRM events ||
|| [Online Store](./sale/index.md) | Orders, cart, payments, shipments, cash registers, and store events ||
|| [Product Catalog](./catalog/index.md) | Products, offers, prices, warehouses, warehouse documents, and catalog events ||
|| [Payment Systems](./pay-system/index.md) | Payment processors and payment settings ||
|| [Document Generator](./document-generator/index.md) | Documents, templates, numbering, and roles ||
|| [e-Signature](./sign/index.md) | HR electronic document management, signing documents with employees, integration with HR record-keeping systems, and signing events ||
|#

### Automation and company operations

#|
|| **Section** | **Description** ||
|| [Business Processes and Robots](./bizproc/index.md) | Templates, process launch, tasks, robots, and business process actions ||
|| [Tasks](./tasks/index.md) | Tasks, checklists, comments, templates, stages, flows, and events. REST 3.0: some of the methods — the task itself, its chat, results, files, access permissions, and Gantt chart links ||
|| [Workgroups and Projects](./sonet-group/index.md) | Workgroups, projects, participants, scrum, and group events ||
|| [Calendar](./calendar/index.md) | Calendars, entries, resources, and events ||
|| [Online booking](./booking/index.md) | Resources and their types, slots, bookings, waiting list, clients, links to external systems, and online booking events ||
|| [Event Log](./event-log/index.md) | Employee sign-ins to Bitrix24 and other event log records. REST 3.0: the entire section ||
|| [Company Structure](./departments/index.md) | Departments, teams, participants, and employees. REST 3.0: some of the methods — `humanresources.*`, while the `department.*` methods work in the old version ||
|| [Time Tracking](./timeman/index.md) | Working time, schedules, and time control. REST 3.0: some of the methods — work time records ||
|| [Users](./user/index.md) | Users and user fields ||
|#

### Communications

#|
|| **Section** | **Description** ||
|| [Chats](./chats/index.md) | Chats, messages, files, users, and notifications ||
|| [Bot Platform](./chat-bots/index.md) | Chatbots, commands, messages, and bot events. The current branch is `imbot.v2`, while the previous generation `imbot.*` is no longer developed ||
|| [Open Channels](./imopenlines/index.md) | Open Channels, sessions, operators, chats, connectors, and channel events ||
|| [Telephony](./telephony/index.md) | Calls, Voximplant lines, and telephony events. REST 3.0: some of the methods — call follow-ups ||
|| [Messaging providers, SMS providers](./messageservice/index.md) | Third-party providers for sending SMS and other messages to customers, and delivery statuses ||
|| [News Feed](./log/index.md) | News, comments, and feed events ||
|| [Surveys, polls](./vote/index.md) | Surveys in chats and the news feed, votes, results, and reports ||
|| [E-mail](./mail/index.md) | Mailboxes, messages, and recipients. REST 3.0: the entire section ||
|| [Email Services](./mailservice/index.md) | Email services with IMAP parameters used to connect Gmail, Outlook, and other mailboxes ||
|#

### Data, files, and content

#|
|| **Section** | **Description** ||
|| [Drive](./disk/index.md) | Files, folders, data store, versions, and permissions ||
|| [How to work with files](./files/index.md) | File transfer formats in REST API ||
|| [Data store](./entity/index.md) | The application's own data: `entity` storages, their sections, items, and properties ||
|| [Universal Lists](./lists/index.md) | Lists, sections, fields, and items ||
|| [Sites and stores](./landing/index.md) | Sites, pages, blocks, templates, and permissions ||
|| [Knowledge Base 2.0](./note/index.md) | Collections, documents, and files. REST 3.0: the entire section ||
|#

### Platform capabilities

#|
|| **Section** | **Description** ||
|| [Widgets](./widgets/index.md) | Application interface embedding points in Bitrix24 ||
|| [BIconnector](./biconnector/index.md) | Connectors, sources, and BI analytics tables ||
|| [BitrixGPT](./ai/index.md) | Custom AI services connected with the `ai.engine.*` methods ||
|| [Vibe](./vibe/index.md) | Custom widgets for the Vibe page, which is replacing the News Feed ||
|| [User agreements](./user-consent/index.md) | Agreements, their texts, and user consents ||
|#
