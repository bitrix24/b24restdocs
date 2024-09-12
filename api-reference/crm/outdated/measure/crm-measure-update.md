# Update Measurement Unit crm.measure.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates an existing measurement unit.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** | Identifier of the measurement unit ||
|| **fields**
[`array`](../../data-types.md) | [Set of fields](./crm-measure-add.md) — an array of the form `array("field_to_update"=>"value"[, ...])`, where the field to update can take values from the method [crm.measure.fields](./crm-measure-fields.md). 

To find out the required format of the fields, execute the method [crm.measure.fields](./crm-measure-fields.md) and check the format of the returned values for those fields 
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"**put_id_here**","fields":{"MEASURE_TITLE":"**put_new_title_here**"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.measure.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"**put_id_here**","fields":{"MEASURE_TITLE":"**put_new_title_here**"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.measure.update
    ```

- JS

    ```js
    var id = prompt("Enter ID");
    var title = prompt("Enter new name for the measurement unit");
    BX24.callMethod(
        "crm.measure.update",
        {
            id: id,
            fields: {
                "MEASURE_TITLE": title
            }
        },
        function (result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $id = ''; // Set the ID here
    $title = ''; // Set the new title here

    $result = CRest::call(
        'crm.measure.update',
        [
            'id' => $id,
            'fields' => [
                'MEASURE_TITLE' => $title
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Learning

- [{#T}](./crm-measure-add.md)
- [{#T}](./crm-measure-get.md)
- [{#T}](./crm-measure-list.md)
- [{#T}](./crm-measure-delete.md)
- [{#T}](./crm-measure-fields.md)