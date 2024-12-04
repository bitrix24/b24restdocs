# Get the list of department field names department.fields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no response in case of an error
- no examples
- request and response in xml; if removed, there will be nothing at all
  
{% endnote %}

{% endif %}

> Scope: [`department`](../scopes/permissions.md)
>
> Who can execute the method: any user

Retrieving the list of department field names. The method has no parameters.

## Code Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'department.fields',
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

{% endlist %}

## Request (xml for clarity of the response)

```
https://my.bitrix24.com/rest/department.fields.xml?auth=7c9d8f00ea0ddd9e02cab3eb2b3bd0d1
```

## Response

> 200 OK

```xml
<response>
    <result>
        <ID>ID</ID>
        <NAME>Department Name</NAME>
        <SORT>Sorting Order</SORT>
        <PARENT>Parent Department</PARENT>
        <UF_HEAD>Manager</UF_HEAD>
    </result>
</response>
```