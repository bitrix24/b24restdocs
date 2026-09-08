# Show Access Permission Selection Dialog BX24.selectAccess

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

```js
BX24.selectAccess(value: array, callback: callable): void;
BX24.selectAccess(callback: callable): void;
BX24.selectAccess(title: string, value: array, callback: callable): void;
```

The method `BX24.selectAccess` displays a standard access permission selection dialog. In the dialog, the user selects employees, departments, workgroups, and other recipient categories, and the application receives their [access codes](../../../api-reference/common/system/access-name.md).

Bitrix24 itself renders the dialog on top of the application frame. The application does not have to assemble the list of recipients or check the permissions to view it: in the dialog, the user sees only the objects they have access to. The dialog requires no scope of its own — it opens the Bitrix24 interface instead of calling the REST API.

The method can be called only from an application embedded in the Bitrix24 frame. If the page is opened outside Bitrix24, the library does not initialize and throws an exception when it is included.

Call the method inside the [BX24.init](../system-functions/bx24-init.md) handler. The library does not defer the dialog itself until the initialization, but the functions that are usually called from the `callback` do not work before it — [BX24.userOption.set](../options/bx24-user-option-set.md), for example.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **value**
[`array`](../../../api-reference/data-types.md) | Array of strings with the [access codes](#codes) to block. Blocked codes are shown in the dialog as inactive: they cannot be selected and do not get into the result.

Codes that do not exist in Bitrix24 are ignored by the dialog: they do not affect the selection and do not cause an error.

The parameter can be omitted: the call `BX24.selectAccess(callback)` opens the dialog with no blocked codes. The default value is an empty array ||
|| **callback***
[`callable`](../../../api-reference/data-types.md) | Callback function that receives the result of the selection [(detailed description)](#callback) ||
|| **title**
[`string`](../../../api-reference/data-types.md) | Dialog title. The library accepts the parameter as the first one but does not pass it to Bitrix24, so the title remains the system one ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Show the dialog without restrictions and output the selected codes:

```js
BX24.init(() => {
    BX24.selectAccess((selected) => {
        selected.forEach((item) => {
            console.log(item.provider, item.id, item.name);
        });
    });
});
```

Block part of the codes and retain the selection in the user configurations, so that the dialog is not shown every time the application is opened:

```js
BX24.init(() => {
    // AU and U1 are already retained, so they cannot be selected again
    BX24.selectAccess(['AU', 'U1'], (selected) => {
        const codes = selected.map((item) => item.id);

        // an empty selection does not overwrite the retained codes
        if (codes.length === 0)
        {
            return;
        }

        BX24.userOption.set('recipients', codes.join(','));
    });
});
```

## Response Handling {#callback}

The dialog does not return the data directly: the result of the selection arrives in the `callback` function as an array of objects. The objects are grouped by the `provider` value, and within a group the order matches the order of selection. Do not rely on the position of an element in the array — identify an object by the `provider` and `id` fields.

If the user closes the dialog — with the cross, the close button, or the Esc key — the `callback` function is not invoked. If the user confirms the selection without checking anything, the `callback` receives an empty array, so check the length of the array before you retain the result.

```json
[
    {
        "provider": "intranet",
        "id": "IU1",
        "name": "Klaus Weber"
    },
    {
        "provider": "socnetgroup",
        "id": "SG4_K",
        "name": "Sales Department: All group members"
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
|| **provider**
[`string`](../../../api-reference/data-types.md) | Source of the access code. Possible values:

- `intranet` — employees and departments from the organizational structure, the codes `IU`, `D`, `DR`
- `socnetgroup` — workgroups and projects, the codes `SG<id>_A`, `SG<id>_E`, `SG<id>_K`
- `user` — employees from the general list of users, the code `U`. This tab is present in the dialog only if the current user has the permission to view all users
- `other` — general recipient categories: the codes `AU`, `G2`, `CR`, and the code `U` of the current user

In the response of the [access.name](../../../api-reference/common/system/access-name.md) method, the `provider_id` field matches these values for `intranet` and `socnetgroup`, while the `U<id>` code arrives there with the `provider_id` `user` ||
|| **id**
[`string`](../../../api-reference/data-types.md) | Access code [(detailed description)](#codes) ||
|| **name**
[`string`](../../../api-reference/data-types.md) | Name of the access permission as the user sees it. For employees and general recipient categories — the name as is, for example `Klaus Weber`. For departments, workgroups, and projects — the name with the role after a colon, for example `Sales Department: All group members` ||
|#

#### Access Codes {#codes}

#|
|| **Code** | **Meaning** ||
|| `IU1` | Employee with identifier 1 and their supervisors in the organizational structure ||
|| `U1` | Only the employee with identifier 1, without supervisors. This is how the current user is returned ||
|| `D5` | All employees of the department with identifier 5 ||
|| `DR5` | All employees of the department with identifier 5 and of its child departments ||
|| `SG4_A` | Owner of the workgroup or project with identifier 4 ||
|| `SG4_E` | Moderators of the workgroup or project with identifier 4 ||
|| `SG4_K` | All members of the workgroup or project with identifier 4 ||
|| `AU` | All authorized users ||
|| `G2` | All users, including unauthorized ones. In the interface, such a code is labeled as all visitors ||
|| `CR` | Author of the object ||
|#

A workgroup cannot be selected as a whole: in the dialog, the user selects a role in the group, so the code always arrives with the `_A`, `_E`, or `_K` suffix.

The numeric part of a code is the identifier of the object in Bitrix24. From `IU1` and `U1` you get `ID = 1` for the methods that work with a user, for example for [user.get](../../../api-reference/user/user-get.md).

The whole code is passed to the parameters of the REST API methods that accept access permissions. In the `ACCESS` parameter of the [entity.rights](../../../api-reference/entity/entities/entity-rights.md) method, for example, the code becomes the key of an object, and the value is the permission level: `{"U1": "W", "AU": "R"}`.

The set of available codes depends on the particular Bitrix24 account: the application dialog has no tab with user groups, and the content of the other tabs is defined by the installed modules and by the permissions of the current user. The names of the codes are returned by the [access.name](../../../api-reference/common/system/access-name.md) method — it queries the Bitrix24 access permission providers and recognizes the `IU`, `D`, `DR`, and `SG` codes among others.

## Error Handling

The dialog returns no error codes. The cases when there is no result are covered in the [Response Handling](#callback) section.

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-select-user.md)
- [{#T}](./bx24-select-users.md)
- [{#T}](./bx24-select-crm.md)
- [{#T}](../../../api-reference/common/system/access-name.md)
- [{#T}](../options/bx24-user-option-set.md)
