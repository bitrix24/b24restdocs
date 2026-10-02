# Get Bitrix24 Address BX24.getDomain

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.getDomain(): string;
```

The `BX24.getDomain` method returns the address of the Bitrix24 instance where the application is open, for example `mycompany.bitrix24.com`. The address does not include the protocol. The address is useful when a link to Bitrix24 is needed outside of it: for example, to send it in an email or pass it to an external system.

The library receives the address as soon as it loads, so the method also works before [BX24.init](../system-functions/bx24-init.md). The method requires no scope of its own.

To open a Bitrix24 page from the application, you do not need to build the address: pass a path relative to the root, for example `/crm/deal/details/5/`, to the [BX24.openPath](./bx24-open-path.md) method.

## Method Parameters

No parameters.

## Code Example

{% include [Example Notes](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    const domain = BX24.getDomain();
    console.log(domain); // mycompany.bitrix24.com
});
```

## Response Handling

The method synchronously returns a result of type `string`. Example result:

```json
"mycompany.bitrix24.com"
```

### Returned Data

#|  
|| **Name**  
`type` | **Description** ||  
|| **result**  
[`string`](../../../api-reference/data-types.md) | The Bitrix24 address without the protocol. The library removes ports `80` and `443`, while any other port remains in the address, for example `mycompany.com:8080` ||
|#

## Error Handling

The method does not return error codes.

## Continue Learning

- [{#T}](./index.md)
- [{#T}](../system-functions/bx24-init.md)  
- [{#T}](./bx24-get-lang.md)  