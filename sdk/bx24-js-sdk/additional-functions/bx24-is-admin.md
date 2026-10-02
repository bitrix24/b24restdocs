# Check User Permission to Install the Application with BX24.isAdmin

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.isAdmin(): boolean;
```

The `BX24.isAdmin` method checks whether the current user can install this application in Bitrix24. Despite its name, the method returns `true` not only for an administrator:

- a Bitrix24 administrator — always
- an employee whom the administrator has allowed to install applications — if employees are allowed to install this application

The method does not check permissions in CRM, tasks, and other tools: Bitrix24 checks them when the application calls REST methods. To find out whether the user is an administrator, call the [user.admin](../../../api-reference/common/users/user-admin.md) method.

The value comes from Bitrix24 during library initialization, so call the method in the [BX24.init](../system-functions/bx24-init.md) handler. The method requires no scope of its own.

## Method Parameters

No parameters.

## Code Example

{% include [Example Footnote](../../../_includes/examples.md) %}

Show the application settings button only to users who can install it:

```html
<button id="settings" hidden>Settings</button>

<script>
    BX24.init(function () {
        if (BX24.isAdmin()) {
            document.getElementById('settings').hidden = false;
        }
    });
</script>
```

## Response Handling

The method synchronously returns a result of type `boolean`. Example result for an administrator:

```json
true
```

### Returned Data

#|  
|| **Name**  
`type` | **Description** ||  
|| **result**  
[`boolean`](../../../api-reference/data-types.md) | `true` if the user is a Bitrix24 administrator or can install this application themselves. Otherwise, `false` ||
|#

## Error Handling

The method does not return error codes.

#|
|| **Situation** | **What Happens** | **What to Do** ||
|| The method is called before the library is initialized | Returns `false` even for an administrator | Call the method in the [BX24.init](../system-functions/bx24-init.md) handler ||
|| You need to check specifically for administrator rights | The method can also return `true` for an employee who is allowed to install applications | Call the [user.admin](../../../api-reference/common/users/user-admin.md) method ||
|#

## Continue Learning

- [{#T}](./index.md)
- [{#T}](../system-functions/bx24-init.md)  
- [{#T}](../../../api-reference/common/users/user-admin.md)  