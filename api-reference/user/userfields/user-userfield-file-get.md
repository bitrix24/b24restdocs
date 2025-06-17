# Get File from User Field user.userfield.file.get

{% note warning "We are still updating this page" %}

The page is hidden from the menu, and the method is not operational.

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- It may be necessary to list parameters or fields in a table.
- The required parameters are not specified.
- Examples in other languages are missing.
- The success response is absent.
- The error response is absent.

{% endnote %}

{% endif %}

> Scope: [`user.userfield`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method allows you to retrieve a file from a user field.

## Example

There is a field `UF_USR_1604998606834` of type file. By calling the [user.current](../user-current.md) method, you can get the file in this field for the current user, where:
- **showUrl** - is the URL that will display the file in the browser if the user is authenticated;
- **downloadData** - the data that should be submitted to this method.

```
[UF_USR_1604998606834] => Array
(
    [id] => 774
    [showUrl] => /bitrix/services/main/ajax.php?action=rest.file.get&SITE_ID=s1&entity=USER&id=1&field=UF_USR_1604998606834&value=774
    [downloadData] => Array
        (
            [id] => 1
            [field] => UF_USR_1604998606834
            [value] => 774
        )
)
```

Webhook request:

{% list tabs %}

- URL request

    ```http
    /rest/1/a2ebx1rfao5pq5cr/user.userfield.file.get?id=1&field=UF_USR_1604998606834&value=774
    ```

{% endlist %}

{% note info "" %}

The method returns the file as content for download, not as json/xml.

{% endnote %}