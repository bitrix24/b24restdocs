# Get Bitrix24 Address BX24.getDomain

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The method `BX24.getDomain` returns the value of `PARAMS.DOMAIN`, which is stored during the library initialization. This is the domain of the current Bitrix24.

```js
String BX24.getDomain()
```

## Parameters

No parameters.

## Code Example

{% include [Example Notes](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    const domain = BX24.getDomain();
    console.log(domain);
});
```

## Response Handling

The method synchronously returns a result of type `string`.

### Returned Data

#|  
|| **Name**  
`type` | **Description** ||  
|| **result**  
[`string`](../../../api-reference/data-types.md) | The Bitrix24 domain without ports `80` and `443`, for example `mycompany.bitrix24.com` ||  
|#

## Continue Learning

- [{#T}](../system-functions/bx24-init.md)  
- [{#T}](./bx24-get-lang.md)  