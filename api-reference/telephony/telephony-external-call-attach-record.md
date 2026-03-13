# Attach a Record to a Completed Call telephony.externalCall.attachRecord

> Scope: [`telephony`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `telephony.externalCall.attachRecord` attaches a record to a completed call and to the CRM activity of the call.

Call this method after [telephony.externalCall.finish](./telephony-external-call-finish.md) when the record is ready.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CALL_ID***
[`string`](../data-types.md) | The identifier of the call from the method [telephony.externalCall.register](./telephony-external-call-register.md).

If the method is called again for the same call, the new record will replace the previously attached one. ||
|| **RECORD_URL**
[`string`](../data-types.md) | The URL of the record on an external server. If this parameter is provided, Bitrix24 will download the file from the link.

It is recommended to use this only if the file is reliably and quickly accessible. ||
|| **FILENAME**
[`string`](../data-types.md) | The name of the record file.

Possible extensions:
- `wav`
- `mp3`

In `RECORD_URL` mode:
- if `FILENAME` is not provided, the name is taken from the URL. ||
|| **FILE_CONTENT**
[`string`](../data-types.md) | The file in [Base64](../files/how-to-upload-files.md) encoding. ||
|#

{% note info "" %}

If `FILENAME` is provided but `FILE_CONTENT` is not, the method will return `uploadUrl` and `fieldName` for subsequent file content upload in a separate request.

If both `FILENAME` and `FILE_CONTENT` (or `RECORD_URL`) are provided, the file is attached immediately, and `FILE_ID` is returned in the response.

{% endnote %}

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CALL_ID":"externalCall.716f1cb73def9700a23842adf9c4c568.1773130779","FILENAME":"call-001.mp3","FILE_CONTENT":"SUQzAwAAAAAiVVRJVDI...AAAAAAAAAAAAP8="}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/telephony.externalCall.attachRecord
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CALL_ID":"externalCall.716f1cb73def9700a23842adf9c4c568.1773130779","FILENAME":"call-001.mp3","FILE_CONTENT":"SUQzAwAAAAAiVVRJVDI...AAAAAAAAAAAAP8=","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/telephony.externalCall.attachRecord
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'telephony.externalCall.attachRecord',
            {
                CALL_ID: 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
                FILENAME: 'call-001.mp3',
                FILE_CONTENT: 'SUQzAwAAAAAiVVRJVDI...AAAAAAAAAAAAP8='
            }
        );
        
        const result = response.getData().result;
        console.log('Record attached:', result);
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
                'telephony.externalCall.attachRecord',
                [
                    'CALL_ID' => 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
                    'FILENAME' => 'call-001.mp3',
                    'FILE_CONTENT' => 'SUQzAwAAAAAiVVRJVDI...AAAAAAAAAAAAP8='
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error attaching record: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "telephony.externalCall.attachRecord",
        {
            CALL_ID: 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
            FILENAME: 'call-001.mp3',
            FILE_CONTENT: 'SUQzAwAAAAAiVVRJVDI...AAAAAAAAAAAAP8='
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'telephony.externalCall.attachRecord',
        [
            'CALL_ID' => 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
            'FILENAME' => 'call-001.mp3',
            'FILE_CONTENT' => 'SUQzAwAAAAAiVVRJVDI...AAAAAAAAAAAAP8='
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

#### If both FILENAME and FILE_CONTENT are provided

HTTP status: **200**

```json
{
    "result": {
        "FILE_ID": 9079
    },
    "time": {
        "start": 1773134738,
        "finish": 1773134740.398239,
        "duration": 2.3982388973236084,
        "processing": 2,
        "date_start": "2026-03-10T12:25:38+01:00",
        "date_finish": "2026-03-10T12:25:40+01:00",
        "operating_reset_at": 1773135338,
        "operating": 1.9444890022277832
    }
}
```

#### If FILENAME is provided without FILE_CONTENT

```json
{
    "result": {
        "uploadUrl": "https://test.bitrix24.com/rest/upload.json?auth=2ae2af690000071b006e2cf2000004f5000007e78a418f2bdef34c831de35c35aaa7ac&token=telephony%7CY2FsbElkPWV4dGVybmFsQ2FsbC43MTZmMWNiNzNkZWY5NzAwYTIzODQyYWRmWRxVVVhdzE%3D%7CInekVhYWE3YWMi.rgOHch62uz5kg9GEslf46sPRmN5MYa/kxBkWcOQtZ/8=",
        "fieldName": "file"
    },
    "time": {
        "start": 1773134240,
        "finish": 1773134240.377464,
        "duration": 0.37746405601501465,
        "processing": 0,
        "date_start": "2026-03-10T12:17:20+01:00",
        "date_finish": "2026-03-10T12:17:20+01:00",
        "operating_reset_at": 1773134840,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | The root element of the response ||
|| **FILE_ID**
[`integer`](../data-types.md) | The identifier of the attached file ||
|| **uploadUrl**
[`string`](../data-types.md) | The URL for uploading the file if `FILE_CONTENT` was not provided ||
|| **fieldName**
[`string`](../data-types.md) | The field name for uploading the file ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "Required parameters are not set. Request should contain either URL or FILENAME parameter"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_CORE` | Required parameters are not set. Request should contain either URL or FILENAME parameter | Neither `RECORD_URL` nor `FILENAME` was provided ||
|| `ERROR_CORE` | Call is not found in the statistic table. It looks like it is not finished yet. | The call was not found in the statistics. Ensure that the call is completed. ||
|| `ERROR_CORE` | File name is empty | Empty `FILENAME` ||
|| `ERROR_CORE` | Wrong file extension. Only wav and mp3 are allowed | Invalid file extension ||
|| `ERROR_CORE` | File content is empty. | Empty `FILE_CONTENT` ||
|| `ERROR_CORE` | File content is not properly encoded. Base64 encoding is expected. | `FILE_CONTENT` was not provided in Base64 format ||
|| `ERROR_CORE` | Server returns HTTP error code {N} | HTTP error when uploading the record via `RECORD_URL` ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./telephony-external-call-register.md)
- [{#T}](./telephony-external-call-finish.md)
- [{#T}](./telephony-call-attach-transcription.md)