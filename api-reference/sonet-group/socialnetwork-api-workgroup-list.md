# Get a list of workgroups socialnetwork.api.workgroup.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- no response in case of error
- no response in case of success
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`socialnetwork`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a list of groups

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **filter** | Corresponds to the arFilter parameter for the API method CSocNetGroup::getList. | ||
|| **select** | An array defining the fields to be selected. Contains a list of fields that should be returned by the method. If the array is empty, the fields ID, SITE_ID, NAME, DESCRIPTION, DATE_CREATE, DATE_UPDATE, DATE_ACTIVITY, ACTIVE, VISIBLE, OPENED, CLOSED, SUBJECT_ID, OWNER_ID, KEYWORDS, IMAGE_ID, NUMBER_OF_MEMBERS, INITIATE_PERMS, SPAM_PERMS, SUBJECT_NAME will be selected. Any fields from the list are allowed in the array. | ||
|| **IS_ADMIN** | When Y is passed, it checks if the current user is an administrator of the social network, and if so, disables permission checks when retrieving groups. | ||
|#

{% include [Footnote about parameters](../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod('socialnetwork.api.workgroup.list', {
        filter: {
            ID: 157,
        },
        select: [ 'ID', 'NAME' ]
    }, result => {
        console.log(result);
    });
    ```

{% endlist %}


{% include [Footnote about examples](../../_includes/examples.md) %}