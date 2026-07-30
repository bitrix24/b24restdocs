# REST Call Peculiarities During Bitrix24 Address Changes

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

New Bitrix24 cloud instances are created with generated addresses in the format `b24-xxx.bitrix24.yy`. Subsequently, users can change this address at any time, subject to certain restrictions. These restrictions depend on the selected subscription plan.

## Why This Is Important to Note

If your application makes a Bitrix24 REST call using an address stored on the application side, a situation may arise where this address is no longer valid.

When accessing an outdated address, Bitrix24 performs a redirect to the new one, but such a redirect must be handled correctly within your code.

You will likely notice nothing when using GET parameters in REST calls, but POST requests are somewhat more complex.

Specifically, if you are using PHP and curl, depending on your settings, a POST request may "magically" turn into a GET request during a redirect. In this case, the parameters passed in the POST request are simply lost.

{% note info %}

These REST API operational peculiarities are already accounted for in the [Bitrix24 SDK](../../sdk/index.md).

{% endnote %}

## Approach 1

When performing a POST request, disable redirects. Receive a 302 request status, extract the new address from the result, and repeat the POST request using the new address.

{% list tabs %}

- Python

    ```python
    response = requests.post(url, allow_redirects=False)
    if response.status_code == 302:
        response = requests.post(response.headers['Location'])
    ```

- PHP

    ```php
    <?php

    $options = [
        'http' => [
            'method' => 'POST',
            'follow_location' => false
        ]
    ];

    $context = stream_context_create($options);
    $response = file_get_contents($url, false, $context);

    $headers = $http_response_header;
    $status_line = $headers[0];
    preg_match('{HTTP\/\S*\s(\d{3})}', $status_line, $match);
    $status_code = $match[1];

    if ($status_code == 302) {
        foreach ($headers as $header) {
            if (stripos($header, 'Location:') === 0) {
                $location = trim(substr($header, 9));
                $response = file_get_contents($location, false, $context);
                break;
            }
        }
    }

    ?>
    ```

{% endlist %}

## Approach 2

Use the `curl_setopt($ch, CURLOPT_POSTREDIR, 3)` option, which will allow you to handle the redirect situation.