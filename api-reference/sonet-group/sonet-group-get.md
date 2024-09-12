# Get a List of Social Network Groups sonet_group.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no response in case of an error
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`sonet`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns an array of social network groups, each containing an array of fields, by calling `CSocNetGroup::GetList()`. Only those groups that are accessible to the user based on permissions are returned.

## Request:

```
https://mydomain.bitrix24.com/rest/sonet_group.get.json?auth=bbc392f317df617d02c942a78ad43aab&ORDER[NAME]=ASC&FILTER[%25NAME]=Sales
```

## Response:

>200 OK

```json
{
"result": [
    {
    "ID": "3",
    "SITE_ID": "s1",
    "NAME": "Sales",
    "DESCRIPTION": "Marketing group for sales",
    "DATE_CREATE": "2013-11-06T07:45:12+04:00",
    "DATE_UPDATE": "2013-11-06T07:45:12+04:00",
    "ACTIVE": "Y",
    "VISIBLE": "Y",
    "OPENED": "N",
    "CLOSED": "N",
    "SUBJECT_ID": "1",
    "OWNER_ID": "1",
    "KEYWORDS": "sales, product, marketing, market",
    "NUMBER_OF_MEMBERS": "1",
    "DATE_ACTIVITY": "2013-11-06T07:45:12+04:00",
    "SUBJECT_NAME": "Working Groups",
    "IMAGE": "https://cdn.bitrix24.com/b211545/socialnetwork/ba9/ba9533b38f60ade077b64f06a60d7082/2.jpg",
    "IS_EXTRANET": "Y"
    }
],
"total": 1
}
```

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ORDER** | Corresponds to the arOrder parameter of the `CSocNetGroup::GetList()` method. ||
|| **FILTER** | Corresponds to the arFilter parameter of the `CSocNetGroup::GetList()` method. ||
|| **IS_ADMIN** | When set to Y, it checks if the current user is an administrator of the social network, and if so, disables permission checks when retrieving groups. ||
|#

{% include [Parameter Notes](../../_includes/required.md) %}

Returns the same fields as `CSocNetGroup::GetList()`, except for `INITIATE_PERMS`, `SPAM_PERMS`, and `IMAGE_ID` (instead, it returns the `IMAGE` field, with the file fields corresponding to `IMAGE_ID`).

## Example

```js
// Get a list of all available social network groups whose names start with the substring "Sales", sorted by name in alphabetical order
BX24.callMethod('sonet_group.get', {
    'ORDER': {
        'NAME': 'ASC'
    },
    'FILTER': {
        '%NAME': 'Sales'
    }
});
```
{% include [Example Notes](../../_includes/examples.md) %}