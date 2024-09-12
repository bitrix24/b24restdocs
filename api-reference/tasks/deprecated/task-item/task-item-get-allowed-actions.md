# Get a List of Actions for the Task task.item.getallowedactions

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns an array of identifiers for the allowed actions on the task.

{% note warning %}

The method is deprecated and not supported. It is recommended to use the methods [tasks.task.*](../../index.md).

{% endnote %}

## Method Parameters

#|
|| **Name** | **Description** ||
|| **TASKID** | Task identifier ||
|#

## Table of Identifiers and Allowed Actions for the Task

#|
|| **Identifier** | **Description** ||
|| `1` | ACTION_ACCEPT ||
|| `2` | ACTION_DECLINE ||
|| `3` | ACTION_COMPLETE ||
|| `4` | ACTION_APPROVE ||
|| `5` | ACTION_DISAPPROVE ||
|| `6` | ACTION_START ||
|| `7` | ACTION_DELEGATE ||
|| `8` | ACTION_REMOVE ||
|| `9` | ACTION_EDIT ||
|| `10` | ACTION_DEFER ||
|| `11` | ACTION_RENEW ||
|| `12` | ACTION_CREATE ||
|| `13` | ACTION_CHANGE_DEADLINE ||
|| `14` | ACTION_CHECKLIST_ADD_ITEMS ||
|| `15` | ACTION_ELAPSED_TIME_ADD ||
|| `16` | ACTION_CHANGE_DIRECTOR ||
|| `17` | ACTION_PAUSE ||
|| `18` | ACTION_START_TIME_TRACKING ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.getallowedactions
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.item.getallowedactions
    ```

- JS

    ```js
    BX24.callMethod(
        'task.item.getallowedactions',
        [13],
        function(result)
        {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.item.getallowedactions',
        [
            'TASKID' => 13
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}