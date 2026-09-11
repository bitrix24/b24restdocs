# Show User Single Selection Dialog BX24.selectUser

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

```js
BX24.selectUser(callback: callable): void;
BX24.selectUser(title: string, callback: callable): void;
```

The method `BX24.selectUser` displays a standard single user selection dialog. The dialog shows only the current employees of the company who belong to at least one department. There are no extranet users, dismissed employees, invited employees with an unactivated account, or service users — chatbots, mail, and connector users — in it.

Bitrix24 itself renders the dialog on top of the application frame, so the application does not have to retrieve the list of employees. The dialog shows the whole organizational structure, without limiting it by the permissions of the current user.

The composition of the dialog depends on who opened the application. If it is an extranet user, they have no organizational structure tab, and the list of employees is limited to their workgroups.

The dialog requires no scope of its own: it opens the Bitrix24 interface instead of calling the REST API.

The method can be called only from an application embedded in the Bitrix24 frame, with the [BX24.js library](../index.md) included.

Call the method inside the [BX24.init](../system-functions/bx24-init.md) handler. The library does not defer the dialog itself until the initialization, but the functions that are usually called from the `callback` do not work before it — [BX24.userOption.set](../options/bx24-user-option-set.md), for example.

An employee cannot be checked in advance, and the list of employees in the dialog cannot be narrowed down.

The single selection dialog is created once and then reused. On a repeat call, the same window opens: the previous selection is checked in it, and the search field holds the name of the selected employee. The window lives on the Bitrix24 page, so reloading the application frame does not reset it. The selection is not carried over to the application — retain it yourself.

To select several employees, use [BX24.selectUsers](./bx24-select-users.md): it displays the same dialog with multiple selection and returns an array of objects instead of a single object.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../../api-reference/data-types.md) | Dialog title in the two-argument call form. The library passes the value to Bitrix24, but the dialog does not use it: the window opens without a title. The parameter does not affect the dialog, and there is no need to pass it ||
|| **callback***
[`callable`](../../../api-reference/data-types.md) | Callback function. It receives one parameter — an object with the data of the selected employee [(detailed description)](#callback). If the function is not passed, the dialog opens and closes on a click, but the result of the selection goes nowhere: no error is raised ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Show the dialog and output the selected employee:

```js
BX24.init(() => {
    BX24.selectUser((user) => {
        console.log(user.id, user.name);
    });
});
```

Retain the selected employee in the user configurations, so that the dialog is not shown every time the application is opened:

```js
BX24.init(() => {
    BX24.selectUser((user) => {
        BX24.userOption.set('assignee', user.id);
    });
});
```

Retrieve the profile of the selected employee with the [user.get](../../../api-reference/user/user-get.md) method through [BX24.callMethod](../how-to-call-rest-methods/bx24-call-method.md) and assemble the link to the avatar. The method requires one of the scopes [`user`, `user_brief`, or `user_basic`](../../../api-reference/scopes/permissions.md#codes), but the `EMAIL` field does not arrive in `user_brief`:

```js
BX24.init(() => {
    BX24.selectUser((user) => {
        const avatar = user.photo
            ? 'https://' + BX24.getDomain() + user.photo
            : '';

        BX24.callMethod('user.get', { ID: Number(user.id) }, (result) => {
            if (result.error())
            {
                console.log(result.error());
                return;
            }

            const employee = result.data()[0];

            if (!employee)
            {
                return;
            }

            console.log(employee.EMAIL, employee.WORK_POSITION, avatar);
        });
    });
});
```

## Response Handling {#callback}

The dialog does not return the data directly: the result of the selection arrives in the `callback` function as an object. The method itself returns `void`, so the selection cannot be awaited with `await` — work with the result inside the `callback`.

The `callback` function is triggered by a click on an employee, the dialog has no separate confirmation button. The dialog can be closed without selecting anything by clicking outside the window — it has no cross, and the Esc key does not close it. In this case, the function is not invoked: the single selection dialog never yields an empty result.

```json
{
    "id": "12",
    "name": "Anna Schmidt",
    "sub": true,
    "sup": false,
    "position": "Sales Manager",
    "photo": "/upload/resize_cache/main/c1c/100_100_2/schmidt.jpg",
    "url": ""
}
```

An employee with no position and no photo:

```json
{
    "id": "7",
    "name": "Lukas Fischer",
    "sub": false,
    "sup": false,
    "position": null,
    "photo": "",
    "url": ""
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../api-reference/data-types.md) | Identifier of the employee. It arrives as a string — cast the value to a number for the Bitrix24 methods that expect a `USER_ID` ||
|| **name**
[`string`](../../../api-reference/data-types.md) | Name of the employee, formatted according to the Bitrix24 configurations ||
|| **sub**
[`boolean`](../../../api-reference/data-types.md) | `true` if the employee works in a department that is subordinate to the current user ||
|| **sup**
[`boolean`](../../../api-reference/data-types.md) | `true` if the employee heads the department of the current user or any department above it ||
|| **position**
[`string`](../../../api-reference/data-types.md) | Position of the employee. If the position is not filled in, an empty string or `null` arrives — check the value for truthiness ||
|| **photo**
[`string`](../../../api-reference/data-types.md) | Path to the reduced copy of the employee avatar, relative to the address of Bitrix24, not to the address of the application. To get a working link, assemble it from `https://`, the domain from [BX24.getDomain](../additional-functions/bx24-get-domain.md), and this path. If there is no photo, an empty string arrives ||
|| **url**
[`string`](../../../api-reference/data-types.md) | Link to the profile of the employee. In an application dialog, it always arrives as an empty string ||
|#

Apart from `id`, the fields are returned by the dialog for displaying in the application interface — the up-to-date data about an employee is returned by [user.get](../../../api-reference/user/user-get.md).

## Error Handling

The dialog returns no error codes. The case when there is no result is covered in the [Response Handling](#callback) section.

The only error appears before the dialog is called. The library takes the address of Bitrix24 and the identifier of the application session from `window.name`, which Bitrix24 sets. If the page is opened outside the frame of a Bitrix24 application, the exception with the text `Unable to initialize Bitrix24 JS library!` is thrown when the library script is loaded, and the `BX24` object is nulled. Catch the exception at the point of inclusion: the library cannot be used after that.

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-select-users.md)
- [{#T}](./bx24-select-access.md)
- [{#T}](./bx24-select-crm.md)
- [{#T}](../how-to-call-rest-methods/bx24-call-method.md)
- [{#T}](../../../api-reference/user/user-get.md)
- [{#T}](../options/bx24-user-option-set.md)
