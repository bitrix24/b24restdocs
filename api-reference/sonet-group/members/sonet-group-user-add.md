# Add Users to Group sonet_group.user.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no response in case of error
- no response in case of success
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`sonet`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method allows adding users as members of a workgroup without the need for invitations and confirmations. To perform this operation, the current user must have administrator rights in the social network. If an extranet user is added, the group will become accessible in the extranet (if it was not accessible before).

## Call Parameters

#|
|| **Parameter** | **Description** ||
|| **GROUP_ID** | ID of the workgroup. ||
|| **USER_ID** | ID of the user (or array of IDs) being added to the group. ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    // Adding users with ID=10 and 21 to the social network group with ID=15
    BX24.callMethod('sonet_group.user.add', {
        GROUP_ID: 15,
        USER_ID: [ 10, 21 ]
    });
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}