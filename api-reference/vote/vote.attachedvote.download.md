# Download the voting report vote.AttachedVote.download

> Scope: [`vote`](../scopes/permissions.md)
>
> Who can execute the method: user with read access permission for voting

The method `vote.AttachedVote.download` generates and provides a downloadable report for the vote in the specified format.

{% note info "Method Features" %}

Attention! This method is an exception to the general rule of working with the REST API. Unlike other methods, it returns not a `JSON` object, but the actual content of the file with HTTP headers that initiate the download in the browser.

Due to this, the standard function `BX24.callMethod()` **cannot** process the response and will throw an error. To work with this method, a direct HTTP request must be made, as shown in the JS code example.

{% endnote %}

## Method Parameters

There are three options for calling the method.

### 1. By the ID of the attached vote

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **attachId***  
[`integer`](../data-types.md) | The ID of the attached vote, which can be obtained using the methods [vote.AttachedVote.get](./vote.attachedvote.get.md) or [vote.AttachedVote.getMany](./vote.attachedvote.getMany.md) ||
|#

### 2. By the entity with the vote

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **moduleId***  
[`string`](../data-types.md) | The module ID, possible values:
- `Im` for a vote in chat,
- `blog` for a vote in the feed ||
|| **entityType***  
[`string`](../data-types.md) | The object type, possible values:
- `Bitrix\\Vote\\Attachment\\ImMessageConnector` for a vote in chat,
- `Bitrix\\Vote\\Attachment\\BlogPostConnector` for a vote in the feed ||
|| **entityId***  
[`integer`](../data-types.md) | The ID of the entity, possible values:
- `id` of the chat message with the vote, which can be obtained using the method [vote.Integration.Im.send](./vote.integration.im.send.md),
- `id` of the post with the vote in the feed, which can be obtained using the method [log.blogpost.get](../log/log-blogpost-get.md) ||
|#

### 3. By the signed ID

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **signedAttachId***  
[`string`](../data-types.md) | The signed ID of the attachment, which can be obtained using the method [vote.AttachedVote.get](./vote.attachedvote.get.md), response parameter `signedAttachId` ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -L \
    -o **put_file_name**.xls \
    -H "Content-Type: application/json" \
    -d '{"attachId":**put_attach_id**}' \
    "https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/vote.AttachedVote.download"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -L \
    -o **put_file_name**.xls \
    -H "Content-Type: application/json" \
    -d '{"attachId":**put_attach_id**, "auth": "**put_access_token_here**"}' \
    "https://**put_your_bitrix24_address**/rest/vote.AttachedVote.download"
    ```

- BX24.js

    ```js
    function downloadVoteReportById(attachId)
    {
        // 1. Get authorization data from the BX24 library
        const auth = BX24.getAuth();
        if (!auth)
        {
            return;
        }
    
        // 2. Form the URL for the direct request to the REST API
        const restUrl = new URL(`https://${auth.domain}/rest/vote.AttachedVote.download`);
        restUrl.searchParams.append('auth', auth.access_token);
        restUrl.searchParams.append('attachId', attachId);
    
        console.log(`Download request: ${restUrl}`);
    
        // 3. Execute the request and process the response as a binary file (blob)
        fetch(restUrl)
            .then(response => {
                if (!response.ok)
                {
                    // If the request failed, try to process the standard JSON error from Bitrix24
                    return response.json().catch(() => {
                        // If the response body is not JSON, throw a general network error
                        throw new Error(`Network error: ${response.status} ${response.statusText}`);
                    }).then(errorData => {
                        // If able to parse the JSON error
                        throw new Error(`API error: ${errorData.error_description || 'Unknown error'}`);
                    });
                }
                // On success, get the data as a binary object
                return response.blob();
            })
            .then(blob => {
                // 4. Create an "invisible" link and initiate the download in the browser
                const url = window.URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.style.display = 'none';
                a.href = url;
                
                // Set the file name that the user will see
                a.download = `vote_report_${attachId}.${fileType}`;
                
                document.body.appendChild(a);
                a.click();
                
                // Clean up temporary data
                window.URL.revokeObjectURL(url);
                document.body.removeChild(a);
            })
            .catch(error => {
                console.error('Error downloading the report:', error);
                alert(`Failed to download the report: ${error.message}`);
            });
    }
    ```

- PHP CRest

    ```php
    <?php
    // This file usually contains constants for connection or autoloader settings
    require_once('src/crest.php');

    /**
    * Function to download the report using a direct HTTP request,
    * since the method vote.AttachedVote.download returns not JSON, but the content of the file.
    *
    * @param array $params - Parameters for the REST method (e.g., ['attachId' => 1])
    */
    function downloadVoteReport(array $params, string $saveToFile): bool
    {
        // 1. Get authorization settings. 
        // CRest::getAppSettings() will return either data for OAuth or for webhook.
        // In crest.php, change the access modifier to public for this method
        $authData = CRest::getAppSettings();

        if (empty($authData)) {
            echo "Error: failed to get authorization settings. Check crest.php/settings.php.\n";
            return false;
        }

        // 2. Define the URL for the request
        if (!empty($authData['is_web_hook']) && $authData['is_web_hook'] === 'Y') {
            // Case with webhook
            $url = $authData['client_endpoint'] . 'vote.AttachedVote.download';
            $queryParams = $params;
        } else {
            // Case with OAuth application
            $url = $authData['client_endpoint'] . 'vote.AttachedVote.download';
            $params['auth'] = $authData['access_token'];
            $queryParams = $params;
        }

        $url .= '?' . http_build_query($queryParams);
        echo "Request URL: " . $url . "\n";

        // 3. Execute the request using cURL
        $curl = curl_init();
        curl_setopt_array($curl, [
            CURLOPT_URL => $url,
            CURLOPT_HEADER => false,        // Do not include headers in the response
            CURLOPT_RETURNTRANSFER => true, // Return the response as a string, not output to the browser
            CURLOPT_USERAGENT => 'CRest based downloader',
            CURLOPT_FOLLOWLOCATION => true, // Follow redirects
        ]);

        $response = curl_exec($curl);
        $httpCode = curl_getinfo($curl, CURLINFO_HTTP_CODE);
        $error = curl_error($curl);
        curl_close($curl);

        // 4. Check the result and save the file
        if ($error) {
            echo "cURL error: " . $error . "\n";
            return false;
        }

        if ($httpCode >= 400) {
            echo "Server returned HTTP error " . $httpCode . ".\n";
            // Attempt to decode the error if it's in JSON format
            $errorData = json_decode($response, true);
            if ($errorData && isset($errorData['error_description'])) {
                echo "Error description: " . $errorData['error_description'] . "\n";
            } else {
                echo "Response body: " . $response . "\n";
            }
            return false;
        }
        
        // If all is well, save the response to a file
        if (file_put_contents($saveToFile, $response)) {
            echo "File successfully saved to: " . $saveToFile . "\n";
            return true;
        } else {
            echo "Failed to save file to: " . $saveToFile . "\n";
            return false;
        }
    }


    // --- Example usage ---

    $attachId = 1;
    $fileType = 'xls';
    $fileName = "vote_report_{$attachId}.{$fileType}";

    $result = downloadVoteReport(
        [
            'attachId' => $attachId,
        ],
        $fileName
    );

    if ($result) {
        echo "Task completed.\n";
    } else {
        echo "Errors occurred during execution.\n";
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200 OK**

In case of successful execution, the server returns not a JSON object, but the actual content of the file with HTTP headers that initiate the download in the browser `Content-Disposition: attachment`.

## Error Handling

HTTP status: **4xx**

```json
{
    "error": "ATTACH_NOT_FOUND",
    "error_description": "Attach not found"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ATTACH_NOT_FOUND` | Vote not found ||
|| `ATTACH_READ_ACCESS_DENIED` | No permission to participate in the vote ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./vote.attachedvote.get.md)
- [{#T}](./vote.attachedvote.getAnswerVoted.md)
- [{#T}](./vote.attachedvote.getMany.md)
- [{#T}](./vote.attachedvote.getWithVoted.md)
- [{#T}](./vote.attachedvote.recall.md)
- [{#T}](./vote.attachedvote.resume.md)
- [{#T}](./vote.attachedvote.stop.md)
- [{#T}](./vote.attachedvote.vote.md)
- [{#T}](./vote.integration.im.send.md)