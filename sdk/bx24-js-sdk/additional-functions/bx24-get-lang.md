# Get the Language Identifier of the Current Account BX24.getLang

The method `BX24.getLang` returns the language identifier of the current account. This method works after [BX24.init](../system-functions/bx24-init.md).

```js
String BX24.getLang()
```

## Parameters

No parameters.

## Code Example

```js
BX24.init(function () {
    const lang = BX24.getLang();
    BX24.loadScript('lang/' + lang + '.js', function () {
        console.log('Loading completed');
    });
});
```

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Response Handling

The method synchronously returns a result of type `string`.

### Returned Data

#|  
|| **Name**  
`type` | **Description** ||  
|| **result**  
[`string`](../../../api-reference/data-types.md) | Language identifier of the current account: `ja`, `id`, `ms`, `de`, `la`, `fr`, `it`, `pl`, `br`, `vn`, `tr`, `kz`, `de`, `en`, `ua`, `ar`, `th`, `sc`, `tc` ||  
|#  

## Continue Learning

- [{#T}](../system-functions/bx24-init.md)  
- [{#T}](./bx24-is-admin.md)  