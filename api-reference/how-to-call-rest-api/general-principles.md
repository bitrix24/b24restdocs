# How a Request is Executed

As already mentioned in the article about [authorization in REST API](./authorization.md), a request to a REST API method is an HTTP request to a specific address of a particular Bitrix24 instance in the following format:

```http
https://your-domain.bitrix24.com/rest/method-name?param1=value1&param2=value2....
```

Most methods support passing an array of parameters as a `POST` request in `JSON` format. All methods support the `GET` protocol, and `POST` in `multipart/form-data` format.

In response to a request, the REST API returns the result in `JSON` or `XML` format depending on what the user requested, containing meaningful data or [error information](../../error-codes.md). We recommend trying to [make a simple request](../../first-rest-api-call.md) before you start exploring the Bitrix24 REST API in depth.

## Request Result

The default response format is `JSON`, but it is possible to receive a response in `XML` format if needed. To do this, you need to append the desired format to the REST method name: `.json` or `.xml`.

Pay attention to the result of [batch](./batch.md) in both variations:

{% list tabs %}

- batch (json)

    Request:

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
            "halt": 0,
            "cmd": {
                "get_user": "user.current"
            }
        }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/batch
    ```
    
    Result:

    ```json
    {
        "result": {
            "result": {
                "get_user": {
                    "ID": "1",
                    "ACTIVE": true,
                    "NAME": "Administrator",
                    "LAST_NAME": "Fusion",
                    "EMAIL": "info@efusion.com",
                    "LAST_LOGIN": "2024-08-29T10:29:54+03:00",
                    "DATE_REGISTER": "2023-08-24T03:00:00+03:00",
                    "IS_ONLINE": "Y",
                    "TIME_ZONE_OFFSET": "0",
                    "TIMESTAMP_X": "24.08.2023 13:19:39",
                    "LAST_ACTIVITY_DATE": "2024-08-29 10:59:12",
                    "PERSONAL_GENDER": "",
                    "PERSONAL_BIRTHDAY": "",
                    "UF_EMPLOYMENT_DATE": "",
                    "UF_DEPARTMENT": [
                        1
                    ]
                }
            },
            "result_error": [],
            "result_total": [],
            "result_next": [],
            "result_time": {
                "get_user": {
                    "start": 1724918931.686765,
                    "finish": 1724918931.689633,
                    "duration": 0.0028679370880126953,
                    "processing": 0.0027151107788085938,
                    "date_start": "2024-08-29T11:08:51+03:00",
                    "date_finish": "2024-08-29T11:08:51+03:00"
                }
            }
        },
        "time": {
            "start": 1724918931.634301,
            "finish": 1724918931.689674,
            "duration": 0.05537295341491699,
            "processing": 0.0031290054321289062,
            "date_start": "2024-08-29T11:08:51+03:00",
            "date_finish": "2024-08-29T11:08:51+03:00"
        }
    }
    ```
- batch (xml)

    Request: 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
            "halt": 0,
            "cmd": {
                "get_user": "user.current"
            }
        }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/batch.xml
    ```

    Result:
    ```xml
    <response>
        <result>
            <result>
                <get_user>
                    <ID>1</ID>
                    <ACTIVE>1</ACTIVE>
                    <NAME>Administrator</NAME>
                    <LAST_NAME>Fusion</LAST_NAME>
                    <EMAIL>info@efusion.com</EMAIL>
                    <LAST_LOGIN>2024-08-29T10:29:54+03:00</LAST_LOGIN>
                    <DATE_REGISTER>2023-08-24T03:00:00+03:00</DATE_REGISTER>
                    <IS_ONLINE>Y</IS_ONLINE>
                    <TIME_ZONE_OFFSET>0</TIME_ZONE_OFFSET>
                    <TIMESTAMP_X>24.08.2023 13:19:39</TIMESTAMP_X>
                    <LAST_ACTIVITY_DATE>2024-08-29 10:59:12</LAST_ACTIVITY_DATE>
                    <PERSONAL_GENDER></PERSONAL_GENDER>
                    <PERSONAL_BIRTHDAY></PERSONAL_BIRTHDAY>
                    <UF_EMPLOYMENT_DATE></UF_EMPLOYMENT_DATE>
                    <UF_DEPARTMENT>
                        <item>1</item>
                    </UF_DEPARTMENT>
                </get_user>
            </result>
            <result_error></result_error>
            <result_total></result_total>
            <result_next></result_next>
            <result_time>
                <get_user>
                    <start>1724918984.0348</start>
                    <finish>1724918984.0386</finish>
                    <duration>0.0037539005279541</duration>
                    <processing>0.0035719871520996</processing>
                    <date_start>2024-08-29T11:09:44+03:00</date_start>
                    <date_finish>2024-08-29T11:09:44+03:00</date_finish>
                </get_user>
            </result_time>
        </result>
        <time>
            <start>1724918983.9771</start>
            <finish>1724918984.0386</finish>
            <duration>0.06153392791748</duration>
            <processing>0.0040559768676758</processing>
            <date_start>2024-08-29T11:09:43+03:00</date_start>
            <date_finish>2024-08-29T11:09:44+03:00</date_finish>
        </time>
    </response>
    ```
{% endlist %}


{% note info %}

`JSON` is the primary response format. There is no need to append `.json` to the name of each method.

{% endnote %}