# Tutorials: Ready-Made Use-Cases for the REST API

Tutorials are a practical guide for developers on the Bitrix24 REST API. They help address typical tasks such as creating or modifying entities and setting up integrations with external systems.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

## How to Use the Tutorials

- As educational material for making your first requests. This is suitable for developers who are just starting to work with the Bitrix24 REST API. The tutorials will help you understand how to formulate requests and process responses. For example, the tutorial [How to Attach a Task to a SPA](./tasks/how-to-connect-task-to-spa.md).

- As templates for real scenarios. The tutorials can serve as a foundation for your own solutions: adapt the examples to your needs, add error handling logic, and logging. For instance, the tutorial [How to Implement a Simple Cash Register Using the REST API](./sale/cashbox-add-example.md).

- For quickly solving specific tasks. The tutorials will help you find a ready-made code example that you can use immediately. For example, the tutorial [How to Set Up Rounding for a Custom Number Field](./crm/how-to-add-crm-objects/how-to-add-precision-to-user-field.md).

## Structure of the Tutorials

The tutorials consist of:

- a brief description of the task being solved,
- examples of methods in PHP and JavaScript,
- descriptions of the parameters used,
- examples of server responses in JSON format,
- examples of general code for applications in PHP and JavaScript,
- explanations and tips,
- links to related articles.

## Tips for Beginners

Explore the sections [Getting Started](../first-steps/index.md), [How to Make Your First Request](../first-steps/first-rest-api-call.md), and install the [REST API Documentation app](https://www.bitrix24.com/apps/app/bitrix.restapi/) — it allows you to view documentation and make requests in the application console simultaneously.

Start with simple scenarios, such as filtering data in the tutorial [How to Filter Elements by Stage Name](./crm/how-to-get-lists/how-to-get-elements-by-stage-filter.md). This tutorial does not use methods for modifying or deleting entities, ensuring that the data within the entities remains safe.

Here’s a sample algorithm for working with the section:

1. Select a [category](#categories) from the menu that interests you, such as CRM.
2. Find the tutorial that corresponds to your task.
3. Follow the step-by-step instructions and examples.
4. Adapt the code to your needs.
5. Check the results and ensure everything works.
6. Contact [Bitrix24 support](../bitrix-support.md) if you encounter any difficulties.

## Categories of Tutorials {#categories}

#|
|| [CRM](./crm/index) | How to work with CRM entities: contacts, deals, SPAs, custom fields, widgets, and Sales Intelligence ||
|| [Online Sales and E-commerce](./sale/index) | How to integrate sales functions: create product items, set up delivery, implement a cash register ||
|| [Workflows](./bizproc/index) | How to embed workflow actions, complete processes, configure Automation rules ||
|| [Chatbots](./chat-bots/index) | How to create a chatbot, configure it for Open Channels, create a support channel ||
|| [Telephony](./telephony/index) | How to implement custom scenarios for integration with a call center ||
|| [Tasks](./tasks/index) | How to create tasks with files, attach them to SPAs, add comments ||
|| [Open Channels](./openlines/example-connector) | How to create a connector for online chat on a website and integrate it with Bitrix24 ||
|#

## How to Provide Feedback on the Tutorials

If you would like to add your scenario or improve an existing one:
- submit a [pull request](../change-article.md) with your solution to the documentation repository,
- write an [issue](../support.md) with your idea suggestion.