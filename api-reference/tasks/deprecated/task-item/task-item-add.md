# Add Task task.item.add

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method creates a new task. It returns the identifier of the added task. The following [fields](./index.md) are available.

{% note warning %}

This method is deprecated and not supported. It is recommended to use the methods [tasks.task.*](../../index.md).

{% endnote %}

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **TASKDATA**
[`array`](../../../data-types.md) | Array of data fields for the task (`TITLE`, `DESCRIPTION`, etc.) ||
|#

## Code Examples

{% include [Example Note](../../../../_includes/examples.md) %}

Creating a task.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TITLE":"created via REST API at **current_datetime_here**","RESPONSIBLE_ID":1,"DEADLINE":"2013-05-13T16:06:06+03:00"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TITLE":"created via REST API at **current_datetime_here**","RESPONSIBLE_ID":1,"DEADLINE":"2013-05-13T16:06:06+03:00"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.item.add
    ```

- JS

    ```js
    var dt = new Date();
    BX24.callMethod(
        'task.item.add',
        [{TITLE: 'created via REST API at ' + dt.toLocaleString(), RESPONSIBLE_ID: 1, DEADLINE: '2013-05-13T16:06:06+03:00'}],
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

    $dt = new DateTime();
    $title = 'created via REST API at ' . $dt->format('Y-m-d H:i:s');

    $result = CRest::call(
        'task.item.add',
        [
            'fields' => [
                'TITLE' => $title,
                'RESPONSIBLE_ID' => 1,
                'DEADLINE' => '2013-05-13T16:06:06+03:00'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

Example of recording values with CRM.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":1,"FIELDS":{"UF_CRM_TASK":["L_4","C_7","CO_5","D_10"]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":1,"FIELDS":{"UF_CRM_TASK":["L_4","C_7","CO_5","D_10"]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.item.update
    ```

- JS

    ```js
    BX24.callMethod(
        'task.item.update',
        [1, {UF_CRM_TASK: ["L_4", "C_7", "CO_5", "D_10"]}],
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
        'task.item.update',
        [
            'TASKID' => 1,
            'FIELDS' => [
                'UF_CRM_TASK' => ["L_4", "C_7", "CO_5", "D_10"]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

The numbers are the `ID` of the corresponding values. The value `L_4` indicates a link to the lead task with `ID = 4`. Multiple links of the same type can be specified, for example, `L_4, L_5`.

- `L` — lead
- `C` — contact
- `CO` — company
- `D` — deal