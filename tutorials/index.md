# Tutorials: Ready-to-Use REST API Scenarios

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A tutorial walks through a single practical task: which methods to call, in what order, what data to pass between the calls, and how to verify the result. Tutorials are grouped by common developer tasks: create or update an object, retrieve a list, embed an application interface, and connect Bitrix24 to an external service.

A tutorial does not replace the reference. The parameters, response, and errors of each method are described in the [REST API Reference](../api-reference/index.md).

> Quick navigation: [all tutorial categories](#categories)

## What You Need Before You Start

- **Method call option.** Methods are called through an [inbound webhook](../first-steps/first-rest-api-call.md) if the integration runs within a single Bitrix24, or from an [application](../settings/app-installation/index.md) if the solution is installed on different Bitrix24 accounts. Some scenarios work only in an application — for example, an Open Channels connector and an SMS provider. Their methods require the application context and return an error when called through a webhook
- **Scopes and permissions.** The beginning of a tutorial lists the scopes the application needs and the permissions of the user on whose behalf the methods are executed. Without the required scope, a method returns an error. The full list is on the [Available Scopes in Bitrix24](../api-reference/scopes/permissions.md) page
- **Data for the examples.** Many scenarios create, update, and delete objects. Run the examples on a test Bitrix24 or on test objects. How to substitute your own values in the example code is described on the [How to Use Examples in the Documentation](../first-steps/how-to-use-examples.md) page

## How to Use Tutorials

- **As educational material.** Tutorials show how to form requests and process responses. Start with a simple scenario, such as [How to Filter Items by Stage Name](./crm/how-to-get-lists/how-to-get-elements-by-stage-filter.md) — it only reads data, so the objects in Bitrix24 remain unchanged
- **As a template for your own solution.** Adapt the example to your data, and add error handling and logging. For example, the [Setting Up a Delivery Service for CRM](./sale/delivery-in-crm.md) tutorial
- **As a ready-to-use code snippet.** Find a scenario for a specific task and move the code into your project. For example, the [How to Configure Rounding for a Number Custom Field](./crm/how-to-add-crm-objects/how-to-add-precision-to-user-field.md) tutorial

## Tutorial Structure

Tutorials follow a common outline:

- a brief description of the task, scopes, and permissions
- a list of the scenario methods in call order
- code examples in JS, PHP, and Python
- a description of the parameters to pass
- examples of server responses in JSON format
- explanations, limitations, and links to related articles

## How to Get Started

1. Select a category in the [Tutorial Categories](#categories) table
2. Find the tutorial that solves your task
3. Prepare the identifiers and other data the scenario requires
4. Execute the methods in the order described in the tutorial
5. Verify the result in the Bitrix24 interface

If you have not made API requests yet, start with the [Where to Start](../first-steps/index.md) section.

## Tutorial Categories {#categories}

#|
|| **Category** | **Tasks the Tutorials Cover** ||
|| [CRM](./crm/index.md) | Add and update CRM objects, retrieve lists, pass Sales Intelligence data, embed an application interface into item cards and lists ||
|| [Online Sales](./sale/index.md) | Add a line item with an arbitrary price to an order, set up a delivery service for CRM ||
|| [Product Catalog](./catalog/index.md) | Create a product with custom property values and update those values ||
|| [Business Processes](./bizproc/index.md) | Add a custom workflow action, embed an interface into automation rule parameters, terminate workflows in bulk ||
|| [Chatbots](./chat-bots/index.md) | Create a chatbot, a bot for Open Channels, and a support channel ||
|| [Message Providers](./messageservice/index.md) | Connect an SMS provider and update the message delivery status ||
|| [Mail](./mail/index.md) | Send an e-mail from a connected mailbox, find an incoming e-mail, and create a CRM activity from it ||
|| [Telephony](./telephony/index.md) | Integrate external telephony: register a call, display the call card, attach a call recording ||
|| [Tasks](./tasks/index.md) | Create tasks with files, add comments with attachments, link a task to a smart process ||
|| [Open Channels](./openlines/index.md) | Create a connector for website chat, find a CRM object by dialog ||
|#

## How to Provide Feedback on Tutorials

If you want to add your own scenario or improve an existing one, submit a [Pull Request or an Issue](../feedback.md).

[Contact Bitrix24 Helpdesk](../bitrix-support.md) if a scenario does not work.
