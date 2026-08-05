# How a Request Is Executed

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Accessing the Bitrix24 REST API is based on HTTP requests. Below is a description of how a method request is structured, which HTTP methods and data formats are supported, and the format in which the response is received. Questions regarding [Authorization](authorization.md), [Batch Calls](batch.md), and [Data Encoding](data-encoding.md) are covered on separate pages. After reading this, you will be able to construct a request to any method and select a response format.

## Request Structure

A Bitrix24 REST API method request is an HTTP request to a specific address of a particular type:

```http

https://your-domain.bitrix24.com/rest/method-name?param1=value1&param2=value2....

```

In a real request, in addition to the method parameters, [authorization data](./authorization.md) is passed — either an incoming webhook code or an application OAuth token. Without them, the request will be rejected.

The full address depends on the authorization method:

- **Webhook** — the user identifier and secret code are embedded in the path: `https://your-domain.bitrix24.com/rest/USER_ID/WEBHOOK_CODE/method`
- **OAuth Token** — passed in the `auth` parameter of the request: `https://your-domain.bitrix24.com/rest/method?auth=ACCESS_TOKEN`

Simple requests can be sent via `GET`. To transfer arrays and nested structures use `POST` with a body in `JSON` format — this method is supported by most methods. All methods also accept `GET` and `POST` requests in `multipart/form-data` format. Special characters in parameters must be [encoded](./data-encoding.md) — otherwise, they may break the URL structure, resulting in an incorrect outcome.

In response to a request, the REST API returns meaningful data or [error information](../../error-codes.md). We recommend trying to [execute a simple request](../../first-steps/first-rest-api-call.md) before you begin exploring the Bitrix24 REST API in depth.

## Request Headers {#headers}

For `POST` requests with a `JSON` body, pass these headers:

- `Content-Type: application/json` — specifies the request body format
- `Accept: application/json` — requests the response in `JSON` format
- `User-Agent: integration-name/version` — helps Bitrix24 and intermediate servers correctly identify the HTTP client

For `GET` requests without a body, `Content-Type` is not required. Pass `Accept: application/json` if you expect the method response in `JSON` format.

If a method returns a signed file download link, such as `DOWNLOAD_URL` or `urlMachine`, download the file with a separate `GET` request. In this request, pass these headers:

- `User-Agent: integration-name/version`
- `Accept: */*` or the MIME type of the expected file
- `Accept-Language: en-US,en;q=0.9`
- `Referer: https://your-domain.bitrix24.com/`

Do not change authorization parameters or tokens in the signed link. If the server returns the `Content-Disposition` header with a file name but the response body contains an Nginx `404`, check that the HTTP client passes the file download headers and does not replace `User-Agent` with an empty or technical value.

## Request Result

The default response format is `JSON`, however if necessary you can obtain the answer in the format `XML`. To do this, add the desired format to the method name: `.json` or `.xml`.

`JSON` — the recommended response format: there is no need to append `.json` to the method name, and the format itself is easy to parse in most programming languages. Choose the `XML` format only when required by the receiving party — for example, for legacy integrations or parsers that work exclusively with XML.

The difference between formats can be easily seen using the [batch](./batch.md) method as an example — it executes a batch of requests in a single call. The same request returns the result in `JSON` or `XML` depending on the extension in the method name:

{% list tabs %}

- Response in JSON

    Method `.../batch` returns:

    ```json
    {
        "result": {
            "result": {
                "get_user": {
                    "ID": "1",
                    "NAME": "Klaus",
                    "LAST_NAME": "Weber"
                }
            }
        }
    }
    ```

- Response in XML

    Method `.../batch.xml` returns the same data:

    ```xml
    <response>
        <result>
            <result>
                <get_user>
                    <ID>1</ID>
                    <NAME>Klaus</NAME>
                    <LAST_NAME>Weber</LAST_NAME>
                </get_user>
            </result>
        </result>
    </response>
    ```

{% endlist %}

Only key response fields are shown here. For the full result structure `batch` — including fields `result_error`, `result_total`, `result_next`, and execution time — and a breakdown of the method parameters, see the [batch](./batch.md) page.
