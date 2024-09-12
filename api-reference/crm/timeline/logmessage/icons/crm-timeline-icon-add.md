# Add Icon to crm.timeline.icon.add

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: `administrator`

This method adds a new icon.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **code***
[`string`](../../../../data-types.md) | Icon code (for example, `info`) ||
|| **fileContent***
[`string`](../../../../data-types.md) | Base64 encoded content of the icon file.

File requirements:

- Type — png
- Size — 24x24 pixels
- Background — transparent

||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"code":"info","fileContent":"iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAIAAABvFaqvAAABhWlDQ1BJQ0MgcHJvZmlsZQAAKJF9kT1Iw0AcxV9TRdGqgx1UHDLUgmBBVMRRq1CECqFWaNXB5NIvaNKQpLg4Cq4FBz8Wqw4uzro6uAqC4AeIo5OToouU"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.icon.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"code":"info","fileContent":"iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAIAAABvFaqvAAABhWlDQ1BJQ0MgcHJvZmlsZQAAKJF9kT1Iw0AcxV9TRdGqgx1UHDLUgmBBVMRRq1CECqFWaNXB5NIvaNKQpLg4Cq4FBz8Wqw4uzro6uAqC4AeIo5OToouU","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.icon.add
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.timeline.icon.add",
        {
            code: "info",
            fileContent: "iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAIAAABvFaqvAAABhWlDQ1BJQ0MgcHJvZmlsZQAAKJF9kT1Iw0AcxV9TRdGqgx1UHDLUgmBBVMRRq1CECqFWaNXB5NIvaNKQpLg4Cq4FBz8Wqw4uzro6uAqC4AeIo5OToouU",
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.timeline.icon.add',
        [
            'code' => 'info',
            'fileContent' => 'iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAIAAABvFaqvAAABhWlDQ1BJQ0MgcHJvZmlsZQAAKJF9kT1Iw0AcxV9TRdGqgx1UHDLUgmBBVMRRq1CECqFWaNXB5NIvaNKQpLg4Cq4FBz8Wqw4uzro6uAqC4AeIo5OToouU'
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
    "result": {
        "icon": {
            "code": "info",
            "isSystem": false,
            "fileUri": "/upload/crm/13f/huhnvzds7ckoy6mk5mdze9pb7jqscpxi/e66fm2cbau9f8u32oe9jzx2qflqhj2vv"
        }
    },
    "time": {
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
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
[`object`](../../../../data-types.md) | Root element of the response.

The `result` field contains the [icon](#icon) object ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

#### Icon Object {#icon}

#|
|| **Field**
`type`  | **Description** ||
||**code** | Icon code ||
||**isSystem** | Flag field.

Can have the value:
- `true` — if the icon is standard (provided with the product)
- `false` — if the icon was added by the user 

||
||**fileUri** | Path to the file.

If the icon was added by the user, the field contains the path to the icon image file ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Access denied ||
|| `INVALID_ARG_VALUE` | Invalid `fileContent` parameter specified ||
|| `FILE_SAVE_ERROR` | Unable to save the provided icon file ||
|| `100` | Required fields not provided ||
|| `0` | Other errors (e.g., fatal) ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-timeline-icon-get.md)
- [{#T}](./crm-timeline-icon-list.md)
- [{#T}](./crm-timeline-icon-delete.md)