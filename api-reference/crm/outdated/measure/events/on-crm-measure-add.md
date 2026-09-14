# Event for Adding a New Unit of Measurement onCrmMeasureAdd

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: `any user`

The event `onCrmMeasureAdd` is triggered after a new unit of measurement is added to the account.

## Response Handling

HTTP status: **200**

Returns an array:

```php
array('FIELDS' => array('ID' => $id))
```

where `$id` is the identifier of the created unit of measurement.

## Error Handling

HTTP status: **40x**, **50x**

In case of errors, the exception `\Bitrix\Rest\RestException` is thrown.