# What the Bitrix24 REST API Allows

Bitrix24 provides developers with tools to extend functionality and integrate with external services through the REST API.

With the REST API, you can:

- create your own solutions
- automate processes
- enhance interactions with clients and employees

Below are the key capabilities for development on the platform.

## Automation

Add your own automation tools to Bitrix24: Automation rules, triggers, workflows, Smart scripts, and event handlers. They simplify routine tasks, reduce errors, and speed up data handling.

### Automation Rules and Triggers in CRM

[Automation rules](./api-reference/bizproc/bizproc-robot/index.md) perform routine actions: sending e-mails, creating documents, and changing company or contact details.

[Triggers](./api-reference/crm/automation/index.md) monitor client actions and activate automation rules. For example, a trigger will move a deal to the next stage immediately after payment is made through an external service.

## Workflows

[Workflows](./api-reference/bizproc/index.md) automate the approval of documents, such as vacation requests. This speeds up the processing of requests and improves work organization.

Developers link these processes with external systems, such as time tracking or project management systems.

### Smart Scripts

[Smart scripts](./settings/app-installation/smart-scripts-installation.md) are sequences of automation rules. They pass data to each other and report intermediate results.

Scripts operate within deals, leads, invoices, contacts, and companies. For instance, a script analyzes client behavior on the website and sends personalized offers.

Developers can add their own automation rules and use them in Smart scripts.

### Event Handlers

[Event handlers](./api-reference/events/index.md) respond to actions in Bitrix24: adding a contact, changing data in a deal.

This allows developers to create integrations with external services: analytics, email marketing, ERP systems. Integrations update data, notify employees of events, and synchronize information between platforms.

## Communication with Clients and Employees

Integrate chatbots, messenger connectors, SMS providers, and telephony into Bitrix24. This speeds up request processing. Clients can communicate through their preferred channels.

### Chatbots

[Chatbots](./api-reference/chat-bots/index.md) operate in both internal and external communications. They answer frequently asked questions, conduct surveys, gather information, and relay it to operators.

Within the company, chatbots assist employees in finding information in knowledge bases, reminding them of tasks and events.

### Open Channels Connectors

Messages from Telegram, WhatsApp, Facebook Messenger come into Bitrix24 through [Open Channels connectors](./api-reference/imopenlines/imconnector/index.md). This increases client engagement and ensures requests are not lost.

### SMS Providers

[Providers](./api-reference/messageservice/index.md) send SMS from Bitrix24: notifications about order status, reminders about events, advertisements. This works where there is no internet.

### Telephony

Integration with [telephony](./api-reference/telephony/index.md) automates calls, records conversations, tracks statistics, and creates deals or tasks based on incoming calls. This improves service and enhances sales efficiency.

## Business Content

Use ready-made templates for [landing pages, websites, and online stores](./api-reference/landing/index.md) to reduce development costs.

### Ready-made Landing Pages

Ready-made landing pages launch promotional pages, product pages, and event registrations. Entrepreneurs and marketers can create them without web development knowledge.

### Ready-made Websites

Ready-made websites are templates for corporate sites, blogs, and portfolios. The templates already include structure and design. You just need to adapt them to your business.

### Ready-made Online Stores

Ready-made online stores enable online commerce without technical complexities. The template is set up for managing products, orders, payments, and delivery.

### Document Templates

[Document templates](./api-reference/document-generator/templates/index.md) simplify the creation of contracts, invoices, and estimates. They standardize document flow and reduce errors.

## Integrations with Payment Systems, Delivery Services, and Online Cash Registers

Integrations facilitate payment for goods and services, simplify logistics and delivery, and ensure accounting and control of financial operations in accordance with legislation.

### Integrations with Payment Systems

Clients can pay for purchases using a convenient [payment method](./api-reference/pay-system/index.md): card, wallet, or online banking. This is particularly relevant for online stores, online courses, and booking services.

### Integrations with Delivery Services

Integration with [delivery services](./api-reference/sale/delivery/index.md) calculates delivery costs, tracks packages, and manages orders. This improves service and optimizes logistics.

## Widgets

You can [add widgets](./api-reference/widgets/index.md), analytics, and dashboards to the Bitrix24 interface. This improves user experience and optimizes workflows.

### Widgets in CRM

Widgets in CRM display additional information about clients: recent purchases, preferences, communication history from social networks or external databases. Managers see the complete picture when working with clients.

### Analytics

Analytical widgets provide data for decision-making: key performance indicators, sales reports, website traffic analytics, and advertising campaign effectiveness — all within the Bitrix24 interface.

### Dashboards

Developers create dashboards for tracking projects, tasks, finances — any aspect of the business that requires monitoring.

### External Services

Widgets integrate external services: email, calendar, time tracking. Data is synchronized, and processes are managed from a single interface.

Built-in solutions personalize the workspace for companies. Developers who understand business needs create in-demand applications.  