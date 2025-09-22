# Tutorials: Ready-made Use Cases for REST API

Tutorials are a practical guide for developers on the Bitrix24 REST API. They help solve typical tasks: creating or modifying entities, setting up integrations with external systems.

## How to Use Tutorials

- As educational material for making your first requests. Suitable for developers who are starting to work with the Bitrix24 REST API. Tutorials will help you understand how to form requests and process responses. For example, the tutorial [How to Attach a Task to a SPA](./tasks/how-to-connect-task-to-spa.md).

- As templates for real scenarios. Tutorials can be used as a basis for your own solutions: adapt the examples to your needs, add error handling logic, logging. For example, the tutorial [How to Implement a Simple Cash Register Using REST API](./sale/cashbox-add-example.md).

- For quickly solving specific tasks. Tutorials will help you find a ready-made code example that you can use immediately. For example, the tutorial [How to Set Up Rounding for a Custom Number Field](./crm/how-to-add-crm-objects/how-to-add-precision-to-user-field.md).

## Structure of Tutorials

Tutorials consist of:

- a brief description of the task being solved,
- examples of methods in PHP and JavaScript,
- descriptions of the parameters used,
- examples of server responses in JSON format,
- examples of general code for applications in PHP and JavaScript,
- explanations and tips,
- links to related articles.

## Tips for Beginners

Explore the sections [Getting Started](../first-steps/index.md), [How to Make Your First Request](../first-steps/first-rest-api-call.md).

Start with simple scenarios, such as filtering data in the tutorial [How to Filter Elements by Stage Name](./crm/how-to-get-lists/how-to-get-elements-by-stage-filter.md). It does not use methods for modifying or deleting entities, so the data within the entities will be safe.

Example algorithm for working with the section:

1. Select a [category](#categories) from the menu that interests you, such as CRM.
2. Find the tutorial that corresponds to your task.
3. Follow the step-by-step instructions and examples.
4. Adapt the code to your needs.
5. Check the result and ensure everything works.
6. Contact [Bitrix24 Support](../bitrix-support.md) if you encounter difficulties.

## Categories of Tutorials {#categories}

#|
|| [CRM](./crm/index) | How to work with CRM entities: contacts, deals, SPAs, custom fields, widgets, and Sales Intelligence ||
|| [Online Sales and E-commerce](./sale/index) | How to integrate trading functions: create product items, set up delivery, implement a cash register ||
|| [Workflows](./bizproc/index) | How to embed actions, complete processes, configure Automation rule parameters ||
|| [CoPilot](./ai/add-joke-prompt) | How to add a custom prompt ||
|| [Chatbots](./chat-bots/index) | How to create a chatbot, configure it for Open Channels, create a support channel ||
|| [Telephony](./telephony/index) | How to implement custom scenarios for integration with a call center ||
|| [Tasks](./tasks/index) | How to create tasks with files, attach them to SPAs, add comments ||
|| [Open Channels](./openlines/example-connector) | How to create a connector for online chat on the site and integrate it with Bitrix24 ||
|#

## How to Provide Feedback on Tutorials

If you want to add your scenario or improve an existing one:
- send a [pull request](../change-article.md) with your solution to the documentation repository,
- write an [issue](../support.md) with your idea suggestion.