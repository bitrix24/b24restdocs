# Get Estimate by ID: crm.quote.get

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "read" access permission for estimates

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [crm.item.get](../universal/crm-item-get.md).

{% endnote %}

The method `crm.quote.get` returns an estimate by its ID.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../data-types.md) | The ID of the estimate.

The ID can be obtained using the methods [crm.quote.list](./crm-quote-list.md) and [crm.quote.add](./crm-quote-add.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Example of retrieving an estimate with `id = 43`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":43}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":43,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.quote.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.quote.get',
    		{
    			id: 43,
    		}
    	);

    	const result = response.getData().result;
    	console.info(result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.get',
                [
                    'id' => 43,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo '<pre>';
        print_r($result);
        echo '</pre>';

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting estimate: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.quote.get',
        {
            id: 43,
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.quote.get',
        [
            'id' => 43,
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
    "result": {
        "ID": "43",
        "TITLE": "Estimate for Furniture Supply",
        "STATUS_ID": "SENT",
        "CURRENCY_ID": "EUR",
        "OPPORTUNITY": "150000.00",
        "TAX_VALUE": "0.00",
        "COMPANY_ID": "1",
        "CONTACT_ID": null,
        "MYCOMPANY_ID": "340",
        "BEGINDATE": "2026-03-13T03:00:00+01:00",
        "CLOSEDATE": "2026-03-20T03:00:00+01:00",
        "ACTUAL_DATE": "2026-03-20T03:00:00+01:00",
        "ASSIGNED_BY_ID": "1",
        "CREATED_BY_ID": "577",
        "MODIFY_BY_ID": "577",
        "DATE_CREATE": "2026-03-13T13:15:37+01:00",
        "DATE_MODIFY": "2026-03-13T16:56:07+01:00",
        "OPENED": "Y",
        "CLOSED": "N",
        "COMMENTS": "Conditions and deadlines clarified",
        "LEAD_ID": null,
        "DEAL_ID": null,
        "QUOTE_NUMBER": "43",
        "CONTENT": null,
        "TERMS": null,
        "PERSON_TYPE_ID": "1",
        "LOCATION_ID": null,
        "CLIENT_TITLE": null,
        "CLIENT_ADDR": null,
        "CLIENT_CONTACT": null,
        "CLIENT_EMAIL": null,
        "CLIENT_PHONE": null,
        "CLIENT_TP_ID": null,
        "CLIENT_TPA_ID": null,
        "UTM_SOURCE": null,
        "UTM_MEDIUM": null,
        "UTM_CAMPAIGN": null,
        "UTM_CONTENT": null,
        "UTM_TERM": null,
        "PARENT_ID_136": null,
        "LAST_COMMUNICATION_TIME": null,
        "LAST_ACTIVITY_BY": "577",
        "LAST_ACTIVITY_TIME": "2026-03-13T13:15:37+01:00",
        "UF_CRM_566EB0AC9B7FC": "",
        "UF_CRM_566EB0AFA9895": "0",
        "UF_CRM_566EB0AFB84F1": "Online Store",
        "UF_CRM_566EB0AFC58FD": "0",
        "UF_CRM_566EB0AFDA4EF": "",
        "UF_CRM_566EB0AFEDB2F": "",
        "UF_CRM_566EB0B014EC2": "",
        "UF_CRM_566EB0B0231B2": "",
        "UF_CRM_566EB0B02F46F": "",
        "UF_CRM_566EB0B03CDCD": "",
        "UF_CRM_566EB0B04D190": "267",
        "UF_CRM_577BBCE34CC90": "",
        "UF_CRM_577BBCE354F86": ""
    },
    "time": {
        "start": 1773411068,
        "finish": 1773411068.183631,
        "duration": 0.18363094329833984,
        "processing": 0,
        "date_start": "2026-03-13T17:11:08+01:00",
        "date_finish": "2026-03-13T17:11:08+01:00",
        "operating_reset_at": 1773411668,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | The root element of the response. Contains fields of the estimate.

The set and types of fields can be obtained using the method [crm.quote.fields](./crm-quote-fields.md) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | `ID is not defined or invalid.` | An incorrect `id` was provided ||
|| `-` | `Access denied.` | The user does not have permission to read estimates ||
|| `-` | `Not found` | The estimate with the provided `id` was not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-quote-add.md)
- [{#T}](./crm-quote-update.md)
- [{#T}](./crm-quote-list.md)
- [{#T}](./crm-quote-delete.md)
- [{#T}](./crm-quote-fields.md)