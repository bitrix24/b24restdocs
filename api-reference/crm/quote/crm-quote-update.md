# Update Commercial Estimate crm.quote.update

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for estimates

{% note warning "Method Development Halted" %}

The method `crm.quote.update` continues to function, but there is a more current alternative, [crm.item.update](../universal/crm-item-update.md).

{% endnote %}

The method `crm.quote.update` updates an existing estimate.

## Method Parameters

{% include [Note on Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../data-types.md) | Identifier of the estimate.

The identifier can be obtained using the methods [crm.quote.list](./crm-quote-list.md) and [crm.quote.add](./crm-quote-add.md) ||
|| **fields**
[`object`](../data-types.md) | Object format:

```json
{
    "field_1": "value_1",
    "field_2": "value_2",
    "...": "..."
}
```

where:
- `field_n` — field name
- `value_n` — new field value

Only include the fields that need to be changed in `fields`.

Unknown fields in `fields` are ignored.

The list of primary fields for updating is provided [below](#parameter-fields).

A complete list of fields and types can be obtained using the method [crm.quote.fields](./crm-quote-fields.md) ||
|| **params**
[`object`](../data-types.md) | Object of additional parameters [(detailed description)](#parameter-params) ||
|#

### Parameter fields {#parameter-fields}

#|
|| **Name**
`type` | **Description** ||
|| **TITLE**
[`string`](../data-types.md) | Subject of the estimate.

Length restriction — up to `255` characters.

If a value longer than `255` is provided, the system will truncate it to `255` characters ||
|| **STATUS_ID**
[`crm_status`](../data-types.md) | Stage of the estimate.

The list of available stages can be obtained using the method [crm.status.list](../status/crm-status-list.md) with the filter `ENTITY_ID = QUOTE_STATUS` ||
|| **CURRENCY_ID**
[`crm_currency`](../data-types.md) | Currency of the estimate amount ||
|| **OPPORTUNITY**
[`double`](../data-types.md) | Amount of the estimate ||
|| **ASSIGNED_BY_ID**
[`user`](../data-types.md) | Identifier of the responsible person ||
|| **COMPANY_ID**
[`crm_company`](../data-types.md) | Identifier of the client company ||
|| **CONTACT_IDS**
[`crm_contact[]`](../data-types.md) | Array of identifiers for client contacts.

The field is completely replaced ||
|| **MYCOMPANY_ID**
[`crm_company`](../data-types.md) | Identifier of "your company" for vendor details ||
|| **OPENED**
[`char`](../data-types.md) | Is the estimate available to everyone? Possible values:
- `Y` — yes
- `N` — no ||
|| **PERSON_TYPE_ID**
[`integer`](../data-types.md) | Identifier of the client type ||
|| **BEGINDATE**
[`date`](../data-types.md) | Date of issue ||
|| **CLOSEDATE**
[`date`](../data-types.md) | Expiration date of the estimate ||
|| **CLIENT_TITLE**
[`string`](../data-types.md) | Client name, up to `255` characters ||
|| **CLIENT_ADDR**
[`string`](../data-types.md) | Client address, up to `255` characters ||
|| **CLIENT_EMAIL**
[`string`](../data-types.md) | Client email, up to `255` characters ||
|| **CLIENT_PHONE**
[`string`](../data-types.md) | Client phone, up to `255` characters ||
|| **COMMENTS**
[`string`](../data-types.md) | Comment ||
|| **PARENT_ID_...**
[`crm_entity`](../data-types.md) | Fields for links to smart processes.

For example, `PARENT_ID_136` — link to the smart process `entityTypeId = 136` ||
|#

{% note info "Method Feature" %}

Some incorrect values in the fields may not lead to a `400` error: values are normalized, truncated, or replaced with default values.

{% endnote %}

### Parameter params {#parameter-params}

#|
|| **Name**
`type` | **Description** ||
|| **REGISTER_HISTORY_EVENT**
[`boolean`](../data-types.md) | Should a record be created in the change history? Possible values:
- `Y` — yes
- `N` — no

Default — `Y` ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

Example of updating an estimate:
- estimate identifier — `43`
- new stage — `SENT`
- updated comment — `Terms and conditions clarified`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":43,"fields":{"STATUS_ID":"SENT","COMMENTS":"Terms and conditions clarified"},"params":{"REGISTER_HISTORY_EVENT":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":43,"fields":{"STATUS_ID":"SENT","COMMENTS":"Terms and conditions clarified"},"params":{"REGISTER_HISTORY_EVENT":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.quote.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.quote.update',
    		{
    			id: 43,
    			fields: {
    				STATUS_ID: 'SENT',
    				COMMENTS: 'Terms and conditions clarified',
    			},
    			params: {
    				REGISTER_HISTORY_EVENT: 'Y',
    			},
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
                'crm.quote.update',
                [
                    'id' => 43,
                    'fields' => [
                        'STATUS_ID' => 'SENT',
                        'COMMENTS' => 'Terms and conditions clarified',
                    ],
                    'params' => [
                        'REGISTER_HISTORY_EVENT' => 'Y',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Updated: ' . ($result ? 'true' : 'false');

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating quote: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.quote.update',
        {
            id: 43,
            fields: {
                STATUS_ID: 'SENT',
                COMMENTS: 'Terms and conditions clarified',
            },
            params: {
                REGISTER_HISTORY_EVENT: 'Y',
            },
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
        'crm.quote.update',
        [
            'id' => 43,
            'fields' => [
                'STATUS_ID' => 'SENT',
                'COMMENTS' => 'Terms and conditions clarified',
            ],
            'params' => [
                'REGISTER_HISTORY_EVENT' => 'Y',
            ],
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
    "result": true,
    "time": {
        "start": 1773410167,
        "finish": 1773410167.690598,
        "duration": 0.6905980110168457,
        "processing": 0,
        "date_start": "2026-03-13T16:56:07+01:00",
        "date_finish": "2026-03-13T16:56:07+01:00",
        "operating_reset_at": 1773410767,
        "operating": 0.26904988288879395
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Root element of the response, returns `true` on success ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Quote is not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | `Parameter 'fields' must be array.` | `fields` is not an object ||
|| `-` | `Parameter 'params' must be array.` | `params` is not an object ||
|| `-` | `ID is not defined or invalid.` | Invalid `id` provided ||
|| `-` | `Access denied.` | User does not have permission to edit estimates ||
|| `-` | `Quote is not found` | Estimate with the provided `id` was not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-quote-add.md)
- [{#T}](./crm-quote-get.md)
- [{#T}](./crm-quote-list.md)
- [{#T}](./crm-quote-delete.md)
- [{#T}](./crm-quote-fields.md)
- [{#T}](./crm-quote-product-rows-set.md)
- [{#T}](./crm-quote-product-rows-get.md)