# Reset Contact Card Settings crm.contact.details.configuration.reset

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method:
>  - Any user has the right to access their own and shared settings
>  - Only an administrator has the right to access others' settings

This method resets the contact card settings: it removes the personal settings of the specified user or the shared settings defined for all users.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | The scope of the settings.

Possible values:
- **P** — personal settings
- **C** — shared settings

Default — `P`
||
|| **userId**
[`user`](../../../data-types.md) | User identifier. Required only when resetting personal settings.

If not specified, the `id` of the current user is used.
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

1. Reset Shared Configuration

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"C"}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.details.configuration.reset
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"C","auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.contact.details.configuration.reset
        ```

    - JS

        ```js
        BX24.callMethod(
            'crm.contact.details.configuration.reset',
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

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.contact.details.configuration.reset',
            [
                'scope' => 'C'
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    {% endlist %}

2. Reset Personal Configuration

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"P","userId":6}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.details.configuration.reset
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"scope":"P","userId":6,"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.contact.details.configuration.reset
        ```

    - JS

        ```js
        BX24.callMethod(
            'crm.contact.details.configuration.reset',
            {
                scope: "P",
                userId: 6,
            },
            (result) => {
                result.error()
                    ? console.error(result.error())
                    : console.info(result.data())
                ;
            },
        );
        ```

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.contact.details.configuration.reset',
            [
                'scope' => 'P',
                'userId' => 6
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
        "start": 1724682584.069094,
        "finish": 1724682584.38436,
        "duration": 0.31526613235473633,
        "processing": 0.025727033615112305,
        "date_start": "2024-08-26T16:29:44+02:00",
        "date_finish": "2024-08-26T16:29:44+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | The root element of the response.

Returns `true` in case of successful settings reset ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description**   | **Value** ||
|| Empty value | Access denied. | The user does not have administrative rights ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-contact-details-configuration-get.md)
- [{#T}](.//crm-contact-details-configuration-set.md)
- [{#T}](./crm-contact-details-configuration-force-common-scope-for-all.md)