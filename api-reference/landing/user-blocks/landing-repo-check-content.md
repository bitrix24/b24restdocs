# Check Content for Dangerous Substrings landing.repo.checkContent

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.repo.checkContent` checks the content for dangerous substrings. These include `onclick=""`, `<iframe>`, and several others. In typical use cases, the chances of triggering are minimal. The method is used solely for content control during [block registration](./landing-repo-register.md).

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **content**
[`unknown`](../../data-types.md) | Content to be tested. ||
|| **splitter**
[`unknown`](../../data-types.md) | Optional parameter for separating dangerous substrings. Defaults to `#SANITIZE#`. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.repo.checkContent',
        {
            content: '<div style="color: red" onclick="alert(123)"><iframe src="//evil.com"></iframe></div>',
            splitter: '#AAA#'
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

# Response on Success

> 200 OK
```json
content:"<div style="color: red" oncl#AAA#ick="alert(123)"><ifr#AAA#ame src="//evil.com"></iframe></div>"
is_bad:true
```

Essentially, the label `is_bad = true` indicates that there are dangerous spots in the content, along with the text marked by separators at those dangerous locations. The developer should modify such spots before registration.