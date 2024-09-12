# Initialize the BX24.init Library

```js
BX24.init(someCallback: function): void;
```

The method executes the `someCallback` function in the context of the application page after the account data has been retrieved. It makes sense to perform all functions that interact with the target account after initialization.

The `BX24.init` function adds an event handler for the "library ready" event. During application initialization, the library requests data for operation from the parent frame. Some actions can only be performed after this data has been received (for example, working with application settings, current user permissions, sending requests to REST, etc.).

## Function Parameters

#|
|| **Name**
`type` | **Description** ||
|| **someCallback**
[`function`](../../data-types.md) | Accepts a function that will be executed upon success ||
|#

## Example

```js
document.addEventListener("DOMContentLoaded", function() {
    BX24.init(function() {
        console.log("BX24 initialized successfully.");

        // Make an API call to fetch current user information
        BX24.callMethod(
            'user.current',
            {},
            function(result) {
                if(result.error()) {
                    console.error("Error fetching user data: ", result.error());
                } else {
                    console.log("User data: ", result.data());
                }
            }
        );
    });
});
```

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Continue Learning

- [{#T}](./bx24-install.md)
- [{#T}](./bx24-install-finish.md)
- [{#T}](./bx24-get-auth.md)
- [{#T}](./bx24-refresh-auth.md)