# Show Multiple User Selection Dialog BX24.selectUsers

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

```js
BX24.selectUsers(callback: callable): void;
BX24.selectUsers(title: string, callback: callable): void;
```

The method `BX24.selectUsers` displays a standard multiple user selection dialog. The dialog shows only the current employees of the company: there are no extranet users and no dismissed employees in it.

Bitrix24 itself renders the dialog on top of the application frame, so the application does not have to retrieve the list of employees. The dialog requires no scope of its own — it opens the Bitrix24 interface instead of calling the REST API. The dialog shows the whole organizational structure, without limiting it by the permissions of the current user.

The method can be called only from an application embedded in the Bitrix24 frame, with the [BX24.js library](../index.md) included.

Call the method inside the [BX24.init](../system-functions/bx24-init.md) handler. The library does not defer the dialog itself until the initialization, but the functions that are usually called from the `callback` do not work before it — [BX24.userOption.set](../options/bx24-user-option-set.md), for example.

The multiple selection dialog is created anew on every call. The employees selected the previous time are not checked in the new dialog — if the selection has to survive between the launches of the application, retain it yourself.

To select a single employee, use [BX24.selectUser](./bx24-select-user.md): it displays the same dialog without multiple selection and returns one object instead of an array.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../../api-reference/data-types.md) | Dialog title in the two-argument call form. The library passes the value to Bitrix24, but the dialog does not use it: the window opens without a title. The parameter is left over from the earlier versions of the library and is not passed in new code ||
|| **callback***
[`callable`](../../../api-reference/data-types.md) | Callback function. It receives one parameter — an array of objects with the data of the selected employees [(detailed description)](#callback) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Show the dialog and output the selected employees:

```js
BX24.init(() => {
    BX24.selectUsers((selected) => {
        selected.forEach((user) => {
            console.log(user.id, user.name);
        });
    });
});
```

Collect the identifiers of the employees and retain them in the user configurations, so that the dialog is not shown every time the application is opened:

```js
BX24.init(() => {
    BX24.selectUsers((selected) => {
        const ids = selected.map((user) => Number(user.id));

        // an empty selection does not overwrite the retained identifiers
        if (ids.length === 0)
        {
            return;
        }

        BX24.userOption.set('assignees', ids.join(','));
    });
});
```

Retrieve the profiles of the selected employees with the [user.get](../../../api-reference/user/user-get.md) method and assemble the links to their avatars. The method requires one of the scopes [`user`, `user_brief`, or `user_basic`](../../../api-reference/scopes/permissions.md). A single `user.get` request returns no more than 50 records — if more employees are selected, read the rest with the `next()` method of the result:

```js
BX24.init(() => {
    BX24.selectUsers((selected) => {
        if (selected.length === 0)
        {
            return;
        }

        const ids = selected.map((user) => Number(user.id));
        const avatars = {};

        selected.forEach((user) => {
            avatars[user.id] = user.photo
                ? 'https://' + BX24.getDomain() + user.photo
                : '';
        });

        BX24.callMethod('user.get', { ID: ids }, (result) => {
            if (result.error())
            {
                console.log(result.error());
                return;
            }

            result.data().forEach((employee) => {
                console.log(employee.EMAIL, avatars[employee.ID]);
            });
        });
    });
});
```

## Response Handling {#callback}

The dialog does not return the data directly: the result of the selection arrives in the `callback` function as an array of objects.

The `callback` function is triggered only by the *Select* button. The dialog can be closed without selecting anything by clicking outside the window — it has no cross, and the Esc key does not close it. In this case, the function is not invoked. If *Select* is pressed with nothing checked, the `callback` receives an empty array — check the length of the array before you retain the result.

The order of the objects in the array does not depend on the order of selection: the employees are sorted by ascending identifier. Identify an employee by the `id` field, not by the position in the array.

```json
[
    {
        "id": "1",
        "name": "Klaus Weber",
        "sub": false,
        "sup": true,
        "position": "Director",
        "photo": "/upload/resize_cache/main/c1c/100_100_2/weber.jpg",
        "url": ""
    },
    {
        "id": "12",
        "name": "Anna Schmidt",
        "sub": true,
        "sup": false,
        "position": "Sales Manager",
        "photo": "",
        "url": ""
    }
]
```

The user confirmed an empty selection:

```json
[]
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../api-reference/data-types.md) | Identifier of the employee. It arrives as a string — cast the value to a number if you pass it to the REST API methods ||
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

The identifier from the `id` field is passed to the Bitrix24 methods that expect a `USER_ID`. The other fields are returned by the dialog for displaying in the application interface — the up-to-date data about an employee is returned by [user.get](../../../api-reference/user/user-get.md).

## Error Handling

The dialog returns no error codes. The cases when there is no result are covered in the [Response Handling](#callback) section.

The only error occurs before the dialog is called. If the page is opened outside the Bitrix24 frame, the exception with the text `Unable to initialize Bitrix24 JS library!` is thrown when the library script is loaded, not when the method is called — catch it at the point of inclusion.

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-select-user.md)
- [{#T}](./bx24-select-access.md)
- [{#T}](./bx24-select-crm.md)
- [{#T}](../how-to-call-rest-methods/bx24-call-method.md)
- [{#T}](../../../api-reference/user/user-get.md)
- [{#T}](../options/bx24-user-option-set.md)
