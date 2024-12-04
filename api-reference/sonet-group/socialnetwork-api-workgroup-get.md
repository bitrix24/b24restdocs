# Get Data for Workgroup socialnetwork.api.workgroup.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

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

> Scope: [`socialnetwork`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns data for the workgroup.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **groupId** | Group identifier. Required parameter, integer. | ||
|#

{% include [Parameter Notes](../../_includes/required.md) %}

## Fields

Available fields:

#|
|| **Field** | **Description** | **Available since** ||
|| **ID** | Group identifier | ||
|| **ACTIVE** | Flag Y/N - is the group active. | ||
|| **SUBJECT_ID** | Subject code (required field). | ||
|| **SUBJECT_DATA** | Fields of the group's subject. | ||
|| **NAME** | Group name | ||
|| **DESCRIPTION** | Group description | ||
|| **KEYWORDS** | Keywords | ||
|| **CLOSED** | Flag Y/N - is the group archived, | ||
|| **VISIBLE** | Flag Y/N - is the group visible in the group list, | ||
|| **OPENED** | Flag Y/N - is the group open for free membership, | ||
|| **PROJECT** | Flag Y/N - is the group a project or not. defaults to - not a project. | 18.0.0 ||
|| **LANDING** | Flag Y/N - is the group for publications. | ||
|| **DATE_CREATE** | Creation date | ||
|| **DATE_UPDATE** | Modification date | ||
|| **DATE_ACTIVITY** | Activity date | ||
|| **IMAGE_ID** | Identifier of the group's avatar file | ||
|| **AVATAR** | Avatar URL | ||
|| **AVATAR_TYPES** | Data about the set of existing avatar types. | ||
|| **AVATAR_TYPE** | Type of the group's avatar (used if IMAGE_ID is not specified). Allowed values: folder, checks, pie, bag, members. | ||
|| **OWNER_ID** | Identifier of the group owner | ||
|| **OWNER_DATA** | Fields of the group owner. | ||
|| **NUMBER_OF_MEMBERS** | Number of group members | ||
|| **NUMBER_OF_MODERATORS** | Number of group moderators. | ||
|| **INITIATE_PERMS** | Who has the right to invite users to the group (required field):
- A - only the group owner
- E - the group owner and group moderators
- K - all group members | ||
|| **PROJECT_DATE_START** | Project start date | ||
|| **PROJECT_DATE_FINISH** | Project completion date | ||
|| **SCRUM_OWNER_ID** | SCRUM identifier | ||
|| **SCRUM_MASTER_ID** | SCRUM master identifier | ||
|| **SCRUM_SPRINT_DURATION** | Duration of the scrum sprint in seconds | ||
|| **SCRUM_TASK_RESPONSIBLE** | Default responsible in the scrum project. available values:
- A - Creator
- M - scrum master | ||
|| **TAGS** | Group tags. ||
|| **ACTIONS** | Data on available operations for the current user on the group. ||
|| **USER_DATA** | Data on the current user's role in the group. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod('socialnetwork.api.workgroup.get', {
        params: {
            groupId: 622,
            select: [ 'ID', 'NAME' ],
        },
    }, result => {
        console.log(result);
    });
    ```

{% endlist %}

{% include [Example Notes](../../_includes/examples.md) %}