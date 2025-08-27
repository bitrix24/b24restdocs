# Delete Digital Workplace crm.automatedsolution.delete

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

This method deletes an existing digital workplace with the identifier `id`.

Deletion of the digital workplace is only possible if there are no bound SPAs associated with it.

If there are SPAs, they must first be unbound or reassigned to another workplace before deleting this digital workplace.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the digital workplace. It can be obtained from the response of the method [crm.automatedsolution.add](./crm-automated-solution-add.md) (`result.automatedSolution.id`), which was called when adding the digital workplace, or [crm.automatedsolution.list](./crm-automated-solution-list.md). You can also use the "Digital Workplaces" section on the Bitrix24 account — the `ID` column in the list of digital workplaces ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":5}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automatedsolution.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":5,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.automatedsolution.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.automatedsolution.delete',
    		{
    			"id": 5
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.automatedsolution.delete',
                [
                    'id' => 5,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Info: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting automated solution: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.automatedsolution.delete",
        {
            "id": 5
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.automatedsolution.delete',
        [
            'id' => 5
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": null,
    "time": {
        "start": 1715852365.744733,
        "finish": 1715852366.078274,
        "duration": 0.3335409164428711,
        "processing": 0.01611018180847168,
        "date_start": "2024-05-16T12:39:25+02:00",
        "date_finish": "2024-05-16T12:39:26+02:00",
        "operating_reset_at": 1715852966,
        "operating": 0
    }
}
```

## Error Handling

HTTP Status: **400**

```json
{	
    "error":"HAS_BOUND_TYPES",
    "error_description":"Cannot delete the workplace that has SPAs. Move them to another workplace"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions ||
|| `HAS_BOUND_TYPES` | The digital workplace has bound SPAs. You must unbind the SPAs before deletion ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-automated-solution-add.md)
- [{#T}](./crm-automated-solution-update.md)
- [{#T}](./crm-automated-solution-get.md)
- [{#T}](./crm-automated-solution-list.md)
- [{#T}](./crm-automated-solution-fields.md)