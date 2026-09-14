# Event for Adding a New Product Section onCrmProductSectionAdd

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `onCrmProductSectionAdd` is triggered after a section is added.

## Handling the Response

HTTP status: **200**

Returns an array:

```php
array('FIELDS' => array('ID' => $id))
```

## Error Handling

HTTP status: **400**

In case of an error or if the section is created **not in the CRM information block**, it throws the exception `\Bitrix\Rest\RestException`.