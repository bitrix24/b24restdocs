# Tutorials: Ready-to-Use REST API Scenarios

Tutorials are a practical guide for developers working with the Bitrix24 REST API. Tutorials help solve common tasks: creating or modifying items, and configuring integrations with external systems.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

## How to Use Tutorials

- As educational material for making your first requests. This is suitable for developers who are starting to work with the Bitrix24 REST API. Tutorials will help you understand how to form requests and process responses. For example, the [How to Attach a Task to a SPA](./tasks/how-to-connect-task-to-spa.md) tutorial.

- As templates for real-world scenarios. Tutorials can be used as a foundation for your own solutions: adapt the examples to your needs, and add error handling logic and logging. For example, the [How to Implement a Simple Cash Register via REST API](./sale/cashbox-add-example.md) tutorial.

- For quickly solving specific tasks. Tutorials help you find ready-to-use code examples. For example, the [How to Configure Rounding for a Custom Numeric Field](./crm/how-to-add-crm-objects/how-to-add-precision-to-user-field.md) tutorial.

## Tutorial Structure

Tutorials consist of:

- a brief description of the task being solved,
- method examples in PHP and JavaScript,
- descriptions of the parameters used,
- examples of server responses in JSON format,
- examples of boilerplate code for PHP and JavaScript applications,
- explanations and tips,
- links to related articles.

## Tips for Beginners

Study the [Getting Started](../first-steps/index.md) and [How to Make Your First Request](../first-steps/first-rest-api-call.md) sections to learn the basics of working with the Bitrix24 REST API.

Start with simple scenarios, such as filtering data in the [How to Filter Items by Stage Name](./crm/how-to-get-lists/how-to-get-elements-by-stage-filter.md) tutorial. It does not use methods to modify or delete items, so the data within the items will remain safe.

Example workflow for working with a section:

1. Select a [category](#categories) of interest from the menu, such as CRM.
2. Find the tutorial that corresponds to your task.
3. Follow the step-by-step instructions and examples.
4. Adapt the code to your needs.
5. Verify the result and ensure everything works correctly.
6. [Contact Bitrix24 Helpdesk](../bitrix-support.md) if you encounter any difficulties.

## Tutorial Categories {#categories}

#|
|| [CRM](./crm/index) | How to work with CRM objects: contacts, deals, smart processes, custom fields, widgets, and Sales Intelligence ||
|| [Online sales and Online store](./sale/index) | How to integrate trading functions: create product items, set up delivery, implement a cash register ||
|| [Product catalog](./catalog/index) | How to create products, properties, prices, and other catalog data ||
|| [Business processes](./bizproc/index) | How to embed an action, complete processes, configure robot parameters ||
|| [Chatbots](./chat-bots/index) | How to create a chatbot, configure it for Open Channels, and create a support channel ||
|| [Telephony](./telephony/index) | How to implement custom scenarios for integration with a call center ||
|| [Tasks](./tasks/index) | How to create tasks with files, attach them to smart processes, and add comments ||
|| [Open Channels](./openlines/example-connector) | How to create a connector for an online chat on a website and integrate it with Bitrix24 ||
|#

## How to Provide Feedback on Tutorials

If you want to add your own scenario or improve an existing one, you can submit [Pull Requests and Issues](../feedback.md).
