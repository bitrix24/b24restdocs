# Add Estimate crm.quote.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "add" access permission for estimates

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [crm.item.add](../universal/crm-item-add.md).

{% endnote %}

The method `crm.quote.add` creates an estimate.

If you need to explicitly specify the details of the buyer and seller in the estimate, use the method [crm.requisite.link.register](../requisites/links/crm-requisite-link-register.md).

## Method Parameters

{% include [Parameter Note](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../data-types.md) | An object containing the fields of the estimate in the following format:

```json
{
    "field_1": "value_1",
    "field_2": "value_2",
    "...": "..."
}
```

where:
- `field_n` — field name
- `value_n` — field value

The list of required and main fields is provided [below](#parameter-fields).

A complete list of available fields and their types can be obtained using the method [crm.quote.fields](./crm-quote-fields.md)
||
|| **params**
[`object`](../data-types.md) | An object of additional parameters [(detailed description)](#parameter-params) ||
|#

### Parameter fields {#parameter-fields}

{% include [Parameter Note](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TITLE***
[`string`](../data-types.md) | Subject of the estimate

Length restriction — up to `255` characters.

If a value longer than `255` is provided, the system will truncate it to `255` characters ||
|| **STATUS_ID**
[`crm_status`](../data-types.md) | Stage of the estimate

Provide the value explicitly in each request.

The list of available stages can be obtained using the method [crm.status.list](../status/crm-status-list.md) with the filter `ENTITY_ID = QUOTE_STATUS` ||
|| **CURRENCY_ID**
[`crm_currency`](../data-types.md) | Currency of the estimate amount ||
|| **OPPORTUNITY**
[`double`](../data-types.md) | Amount of the estimate

Provide a numeric value along with `CURRENCY_ID` ||
|| **ASSIGNED_BY_ID**
[`user`](../data-types.md) | Identifier of the responsible person ||
|| **COMPANY_ID**
[`crm_company`](../data-types.md) | Identifier of the client company ||
|| **CONTACT_IDS**
[`crm_contact[]`](../data-types.md) | Array of identifiers for client contacts
||
|| **MYCOMPANY_ID**
[`crm_company`](../data-types.md) | Identifier of "your company" for seller requisites ||
|| **OPENED**
[`char`](../data-types.md) | Is the estimate available to everyone? Possible values:
- `Y` — yes
- `N` — no ||
|| **PERSON_TYPE_ID**
[`integer`](../data-types.md) | Identifier of the payer type ||
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
|#

{% note info "Method Feature" %}

Some incorrect values in the fields may not lead to a `400` error: values are normalized, truncated, or replaced with default values.

{% endnote %}

### Parameter params {#parameter-params}

#|
|| **Name**
`type` | **Description** ||
|| **IMPORT**
[`boolean`](../data-types.md) | Import mode. Possible values:
- `Y` — yes
- `N` — no ||
|#

**Import**

These fields are available for filling when the parameter `IMPORT = 'Y'` is passed in the `params` parameter.

#|
|| **Name**
`type` | **Description** ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Creation date ||
|| **DATE_MODIFY**
[`datetime`](../data-types.md) | Modification date ||
|| **CREATED_BY_ID**
[`user`](../data-types.md) | Created by ||
|| **MODIFY_BY_ID**
[`user`](../data-types.md) | Modified by ||
|#

## Code Examples

{% include [Example Note](../../../_includes/examples.md) %}

Example of creating an estimate:
- subject — `Estimate for Furniture Supply`
- buyer — company with `id = 1`
- seller — your company with `id = 3`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TITLE":"Estimate for Furniture Supply","STATUS_ID":"DRAFT","OPENED":"Y","ASSIGNED_BY_ID":1,"CURRENCY_ID":"USD","OPPORTUNITY":150000,"COMPANY_ID":1,"MYCOMPANY_ID":3,"COMMENTS":"Prepared upon client request","BEGINDATE":"2026-03-13T10:00:00+01:00","CLOSEDATE":"2026-03-20T18:00:00+01:00"},"params":{"IMPORT":"N"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TITLE":"Estimate for Furniture Supply","STATUS_ID":"DRAFT","OPENED":"Y","ASSIGNED_BY_ID":1,"CURRENCY_ID":"USD","OPPORTUNITY":150000,"COMPANY_ID":1,"MYCOMPANY_ID":3,"COMMENTS":"Prepared upon client request","BEGINDATE":"2026-03-13T10:00:00+01:00","CLOSEDATE":"2026-03-20T18:00:00+01:00"},"params":{"IMPORT":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.quote.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.quote.add',
    		{
    			fields: {
    				TITLE: 'Estimate for Furniture Supply',
    				STATUS_ID: 'DRAFT',
    				OPENED: 'Y',
    				ASSIGNED_BY_ID: 1,
    				CURRENCY_ID: 'USD',
    				OPPORTUNITY: 150000,
    				COMPANY_ID: 1,
    				MYCOMPANY_ID: 3,
    				COMMENTS: 'Prepared upon client request',
    				BEGINDATE: '2026-03-13T10:00:00+01:00',
    				CLOSEDATE: '2026-03-20T18:00:00+01:00',
    			},
    			params: {
    				IMPORT: 'N',
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
                'crm.quote.add',
                [
                    'fields' => [
                        'TITLE' => 'Estimate for Furniture Supply',
                        'STATUS_ID' => 'DRAFT',
                        'OPENED' => 'Y',
                        'ASSIGNED_BY_ID' => 1,
                        'CURRENCY_ID' => 'USD',
                        'OPPORTUNITY' => 150000,
                        'COMPANY_ID' => 1,
                        'MYCOMPANY_ID' => 3,
                        'COMMENTS' => 'Prepared upon client request',
                        'BEGINDATE' => '2026-03-13T10:00:00+01:00',
                        'CLOSEDATE' => '2026-03-20T18:00:00+01:00',
                    ],
                    'params' => [
                        'IMPORT' => 'N',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Created quote ID: ' . $result;

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating quote: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.quote.add',
        {
            fields: {
                TITLE: 'Estimate for Furniture Supply',
                STATUS_ID: 'DRAFT',
                OPENED: 'Y',
                ASSIGNED_BY_ID: 1,
                CURRENCY_ID: 'USD',
                OPPORTUNITY: 150000,
                COMPANY_ID: 1,
                MYCOMPANY_ID: 3,
                COMMENTS: 'Prepared upon client request',
                BEGINDATE: '2026-03-13T10:00:00+01:00',
                CLOSEDATE: '2026-03-20T18:00:00+01:00',
            },
            params: {
                IMPORT: 'N',
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
        'crm.quote.add',
        [
            'fields' => [
                'TITLE' => 'Estimate for Furniture Supply',
                'STATUS_ID' => 'DRAFT',
                'OPENED' => 'Y',
                'ASSIGNED_BY_ID' => 1,
                'CURRENCY_ID' => 'USD',
                'OPPORTUNITY' => 150000,
                'COMPANY_ID' => 1,
                'MYCOMPANY_ID' => 3,
                'COMMENTS' => 'Prepared upon client request',
                'BEGINDATE' => '2026-03-13T10:00:00+01:00',
                'CLOSEDATE' => '2026-03-20T18:00:00+01:00',
            ],
            'params' => [
                'IMPORT' => 'N',
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
    "result": 43,
    "time": {
        "start": 1773396937,
        "finish": 1773396937.557957,
        "duration": 0.5579569339752197,
        "processing": 0,
        "date_start": "2026-03-13T13:15:37+01:00",
        "date_finish": "2026-03-13T13:15:37+01:00",
        "operating_reset_at": 1773397537,
        "operating": 0.5224320888519287
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../data-types.md) | Root element of the response. Contains the identifier of the created estimate ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Parameter 'fields' must be array."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | `Parameter 'fields' must be array.` | `fields` is not an object ||
|| `-` | `Parameter 'params' must be array.` | `params` is not an object ||
|| `-` | `Access denied.` | The user does not have permission to add estimates ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-quote-update.md)
- [{#T}](./crm-quote-get.md)
- [{#T}](./crm-quote-list.md)
- [{#T}](./crm-quote-delete.md)
- [{#T}](./crm-quote-fields.md)
- [{#T}](./crm-quote-product-rows-set.md)
- [{#T}](./crm-quote-product-rows-get.md)