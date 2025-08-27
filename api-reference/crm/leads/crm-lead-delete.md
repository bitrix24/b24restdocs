# Delete lead crm.lead.delete

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user with permission to delete leads

The method `crm.lead.delete` removes a lead and all associated objects: activities, history, timeline records, and others.

Objects are deleted if they are not linked to other objects or entities. If the objects are linked to other entities, only the link to the deleted lead will be removed.

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | The identifier of the lead.

The identifier can be obtained using the methods [crm.lead.list](./crm-lead-list.md) or [crm.lead.add](./crm-lead-add.md) ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

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

    ```js
    try
    {
        const id = prompt("Enter ID");
        const response = await $b24.callMethod(
            'crm.lead.delete',
            { id }
        );
        
        const result = response.getData().result;
        if(result.error())
        {
            console.error(result.error());
            return;
        }
        
        console.info(result);
    }
    catch(error)
    {
        console.error('Error:', error);
    }
    ```

- PHP

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

- BX24.js

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

- PHP CRest

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

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1705764932.998683,
        "finish": 1705764937.173995,
        "duration": 4.1753120422363281,
        "processing": 3.3076529502868652,
        "date_start": "2024-01-20T18:35:32+01:00",
        "date_finish": "2024-01-20T18:35:37+01:00",
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
[`boolean`](../../data-types.md) | The root element of the response, contains `true` in case of success ||
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