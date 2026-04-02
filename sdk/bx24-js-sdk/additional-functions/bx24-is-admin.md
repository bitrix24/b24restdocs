# Check User's Administrator Access with BX24.isAdmin

The method `BX24.isAdmin` determines whether the current user has administrator rights on the account. This method works after [BX24.init](../system-functions/bx24-init.md).

```js
Boolean BX24.isAdmin()
```

## Parameters

No parameters.

## Code Example

{% include [Example Notes](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    const isAdmin = BX24.isAdmin();
    console.log(isAdmin);
});
```

## Response Handling

The method synchronously returns a result of type `boolean`.

### Returned Data

#|  
|| **Name**  
`type` | **Description** ||  
|| **result**  
[`boolean`](../../../api-reference/data-types.md) | `true` if the current user has administrator rights on the account, otherwise `false` ||  
|#

## Continue Learning

- [{#T}](../system-functions/bx24-init.md)  
- [{#T}](../../../api-reference/common/users/user-admin.md)  