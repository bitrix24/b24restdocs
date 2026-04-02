# Get Parameters of crm.lead.details.configuration.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method:
>  - any user can retrieve their own and shared settings,
>  - a user with the "Allow to modify settings" access permission in CRM can retrieve others' settings.

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [crm.item.details.configuration.get](../../universal/item-details-configuration/crm-item-details-configuration-get.md).

{% endnote %}

The method `crm.lead.details.configuration.get` retrieves the settings for lead cards.

{% note warning %}

The settings for repeat leads may differ from those for simple leads. To switch between lead card settings, use the `lead.customer.type` parameter in `extras`.

{% endnote %}

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **userId**
[`user`](../../../data-types.md) | The identifier of the user for whom to retrieve personal configuration.

If the parameter is not provided, the `userId` of the user calling the method will be used.

Required only when requesting personal settings ||
|| **scope**
[`string`](../../../data-types.md) | The scope of the settings. Possible values:
- `'P'` - personal settings
- `'C'` - shared settings

The default value is `'P'` ||
|| **extras**
[`object`](../../../data-types.md) | Additional parameters. The structure is described [below](#extras) ||
|#

### Extras Parameter {#extras}

#|
|| **Name**
`type` | **Description** ||
|| **lead.customer.type**
[`integer`](../../../data-types.md) | Type of lead. Possible values:
- `1` - simple lead
- `2` - repeat lead ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

1. Retrieve Personal Configuration of the Card

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"P","userId":1}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.details.configuration.get
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"P","userId":1,"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.lead.details.configuration.get
        ```

    - JS

        ```js
        try
        {
            const response = await $b24.callMethod(
                'crm.lead.details.configuration.get',
                {
                    scope: 'P',
                    userId: 1,
                }
            );
            
            const result = response.getData().result;
            console.log('Data:', result);
            processResult(result);
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
                    'crm.lead.details.configuration.get',
                    [
                        'scope' => 'P',
                        'userId' => 1
                    ]
                );

            $result = $response
                ->getResponseData()
                ->getResult();

            echo 'Success: ' . print_r($result, true);
            processData($result);

        } catch (Throwable $e) {
            error_log($e->getMessage());
            echo 'Error: ' . $e->getMessage();
        }
        ```

   - BX24.js

       ```js
        BX24.callMethod(
            'crm.lead.details.configuration.get',
            {
                scope: "P",
                userId: 1,
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
            'crm.lead.details.configuration.get',
            [
                'scope' => 'P',
                'userId' => 1
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
       ```

    {% endlist %}

2. Retrieve Shared Configuration of the Card

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"C"}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.details.configuration.get
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"C","auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.lead.details.configuration.get
        ```

    - JS

        ```js
        try
        {
            const response = await $b24.callMethod(
                'crm.lead.details.configuration.get',
                {
                    scope: 'C',
                }
            );
            
            const result = response.getData().result;
            console.log('Configuration details:', result);
            
            processResult(result);
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
                    'crm.lead.details.configuration.get',
                    [
                        'scope' => 'C'
                    ]
                );

            $result = $response
                ->getResponseData()
                ->getResult();

            echo 'Success: ' . print_r($result, true);
            processData($result);

        } catch (Throwable $e) {
            error_log($e->getMessage());
            echo 'Error fetching lead details configuration: ' . $e->getMessage();
        }
        ```

    - BX24.js

        ```js
        BX24.callMethod(
            'crm.lead.details.configuration.get',
            {
                scope: "C",
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
            'crm.lead.details.configuration.get',
            [
                'scope' => 'C'
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
    "result": [
        {
            "name": "main",
            "title": "About the Lead",
            "type": "section",
            "elements": [
                {
                    "name": "TITLE",
                    "optionFlags": "0"
                },
                {
                    "name": "STATUS_ID",
                    "optionFlags": "0"
                },
                {
                    "name": "SOURCE_ID",
                    "optionFlags": "0"
                }
            ]
        },
        {
            "name": "additional",
            "title": "Additional",
            "type": "section",
            "elements": [
                {
                    "name": "ASSIGNED_BY_ID",
                    "optionFlags": "0"
                },
                {
                    "name": "COMMENTS",
                    "optionFlags": "0"
                }
            ]
        }
    ],
    "time": {
        "start": 1720624891.017344,
        "finish": 1720624891.405621,
        "duration": 0.3882770538330078,
        "processing": 0.02097320556640625,
        "date_start": "2024-07-10T17:21:31+02:00",
        "date_finish": "2024-07-10T17:21:31+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`section[]`](#section)\|`null` | The root element of the response. Contains the configuration of the lead card sections. Returns `null` if the configuration is not found ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Section Object {#section}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Unique name of the section ||
|| **title**
[`string`](../../../data-types.md) | Title of the section ||
|| **type**
[`string`](../../../data-types.md) | Type of the section ||
|| **elements**
[`section_element[]`](#section_element) | List of fields displayed in the section ||
|#

#### Section Element Object {#section_element}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Field identifier ||
|| **optionFlags**
[`string`](../../../data-types.md) | Values:
- `"1"` - always show
- `"0"` - not always show ||
|| **options**
[`object`](../../../data-types.md) | Additional options for the field ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | Access denied | Insufficient permissions to retrieve the requested configuration ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-lead-details-configuration-reset.md)
- [{#T}](./crm-lead-details-configuration-set.md)
- [{#T}](./crm-lead-details-configuration-force-common-scope-for-all.md)