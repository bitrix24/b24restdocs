# Get the list of departments department.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- types and requiredness of parameters are not specified
- no response in case of error
- no examples
  
{% endnote %}

{% endif %}

> Scope: [`department`](../scopes/permissions.md)
>
> Who can execute the method: any user

Retrieving a filtered list of departments.

#|
|| **Parameter** | **Description** ||
|| **sort** | the field by which the results are sorted ||
|| **order** | sorting direction: 
- ASC - ascending
- DESC - descending ||
|| **ID** | filter by department ID ||
|| **NAME** | filter by department name ||
|| **PARENT** | filter by parent department ||
|| **UF_HEAD** | filter by department head ||
|| **START** | The ordinal number of the list item from which to return the next items when calling the current method. Details in the article [{#T}](../how-to-call-rest-api/list-methods-pecularities.md) ||
|#

{% include [Footnote about parameters](../../_includes/required.md) %}

Filtering parameters can accept array values.

## Call

```js
BX24.callMethod('department.get', {"ID": 222});
```

## Request

```
https://my.bitrix24.com/rest/department.get.json?ID=222&auth=b961afd4148963c5d54ebeb17123d1fc
```

## Response

> 200 OK

```json
{"result":[{"ID":"222","NAME":"Old Department","SORT":500,"PARENT":"114","UF_HEAD":"11"}],"total":1}
```