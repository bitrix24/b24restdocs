# Register a New User Field Type userfieldtype.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`depending on the embedding location`](../../scopes/permissions.md)
>
> Who can execute the method: any user

Registering a new user field type. The method returns true or an error with a description of the reason.

## Parameters

#|
|| **Parameter** | **Type** | **Description** | **Restrictions** ||
|| **USER_TYPE_ID**^*^
[`String`](../../data-types.md) | String code of the type. Required parameter. | 
- a-z0-9
- Must be unique. ||
|| **HANDLER**^*^
[`URL`](../../data-types.md) | Address of the user type handler. Required parameter. | - in the same domain as the main application address<br>- must be unique ||
|| **TITLE**
[`String`](../../data-types.md) | Text name of the type. Will be displayed in the administrative interface for user field settings. | ||
|| **DESCRIPTION**
[`String`](../../data-types.md) | Text description of the type. Will be displayed in the administrative interface for user field settings. | ||
|| **OPTIONS**
[`Multidimensional array`](../../data-types.md) | Additional settings. Currently, one key is available:<br>**height** - specifies the default height of the user field in pixels. [Default - 0](*default). Any positive value will apply. | ||
|#

\* - required parameters.

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

### Example Call

```js
BX24.callMethod(
    'userfieldtype.add',
    {
        USER_TYPE_ID: 'test',
        HANDLER: 'https://www.myapplication.com/handler/',
        TITLE: 'Test type',
        DESCRIPTION: 'Test user field type for documentation'
    }
);
```

### Example Request

```http
POST https://sometestaccount.bitrix24.com/rest/userfieldtype.add HTTP/1.1

USER_TYPE_ID=test&HANDLER=https%3A%2F%2Fwww.myapplication.com%2Fhandler%2F&TITLE=Test+type&DESCRIPTION=Test+user+field+type+for+documentation&auth=63t6r4z9cugaciaxocrh2r47zlodp12y

HTTP/1.1 200 OK

{
    "result": true
}
```

### Example Using the OPTIONS Parameter:

```php
CRest::call(
    'userfieldtype.add',
    [
        'USER_TYPE_ID' => 'custom_type',
        'HANDLER' => 'https://example.com/field.php',
        'TITLE' => 'title',
        'OPTIONS' => [
            'height' => 60,
        ],
    ]
);
```

{% include [Example Note](../../../_includes/examples.md) %}

[*default] When specifying a value of **0**, the standard height for displaying this embedding will be used.