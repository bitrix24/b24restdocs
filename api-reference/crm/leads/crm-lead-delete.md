# Delete Lead crm.lead.delete

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user with permission to delete leads

The method `crm.lead.delete` removes a lead and all associated objects: tasks, history, timeline records, and others.

Objects are deleted if they are not linked to other objects or entities. If the objects are linked to other entities, only the link to the deleted lead will be removed.

## Method Parameters

{% include [Parameter Note](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the lead.

The identifier can be obtained using the methods [crm.lead.list](./crm-lead-list.md) or [crm.lead.add](./crm-lead-add.md) ||
|#

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"123"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"123","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.lead.delete
    ```

- JS

    ```javascript 
    const id = prompt("Enter ID");
    BX24.callMethod(
      'crm.lead.delete',
      { id },
      (result) => {
        if(result.error())
        {
          console.error(result.error());
  
          return;
        }
        
        console.info(result.data());
      }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $id = readline("Enter ID: ");

    $result = CRest::call(
        'crm.lead.delete',
        [
            'id' => $id
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- PHP (B24PhpSdk)

    ```php        
    try {
        $id = 123; // Example lead ID to delete
        $result = $serviceBuilder
            ->getCRMScope()
            ->lead()
            ->delete($id);
        if ($result->isSuccess()) {
            print("Lead with ID $id has been successfully deleted.");
        } else {
            print("Failed to delete lead with ID $id.");
        }
    } catch (Throwable $e) {
        print("An error occurred: " . $e->getMessage());
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1705764932.998683,
        "finish": 1705764937.173995,
        "duration": 4.1753120422363281,
        "processing": 3.3076529502868652,
        "date_start": "2024-01-20T18:35:32+03:00",
        "date_finish": "2024-01-20T18:35:37+03:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

> 40x, 50x Error

```json
{
  "error": "",
  "error_description": "ID is not defined or invalid."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Description** | **Value** ||
|| `ID is not defined or invalid` | The `id` parameter either has no value or is not a positive integer ||
|| `Access denied` | The user does not have permission to delete leads ||
|| `Not found` | The lead with the provided `id` does not exist ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}