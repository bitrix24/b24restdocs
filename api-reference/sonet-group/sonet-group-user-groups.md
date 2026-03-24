# Get the list of groups for the current user sonet_group.user.groups

> Scope: [`sonet`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `sonet_group.user.groups` returns the groups and projects that the current user is a member of.

## Method Parameters

No parameters.

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sonet_group.user.groups
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sonet_group.user.groups
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'sonet_group.user.groups',
            {}
        );
        
        const result = response.getData().result;
        console.log('Retrieved user groups:', result);
        
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
                'sonet_group.user.groups',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error retrieving user groups: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('sonet_group.user.groups',
        {}, 
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
        'sonet_group.user.groups',
        []
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
    "result": [
        {
        "GROUP_ID": "77",
        "GROUP_NAME": "New Project Title",
        "ROLE": "K",
        "GROUP_IMAGE_ID": null,
        "GROUP_IMAGE": ""
        },
        {
        "GROUP_ID": "79",
        "GROUP_NAME": "Scrum Project",
        "ROLE": "A",
        "GROUP_IMAGE_ID": null,
        "GROUP_IMAGE": ""
        }
    ],
    "time": {
        "start": 1773927027,
        "finish": 1773927028.025164,
        "duration": 1.0251638889312744,
        "processing": 1,
        "date_start": "2026-03-19T16:30:27+02:00",
        "date_finish": "2026-03-19T16:30:28+02:00",
        "operating_reset_at": 1773927627,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | Array of groups the current user belongs to.

An empty array means that the user is not a member of any group or project ||
|| **GROUP_ID**
[`integer`](../data-types.md) | Group identifier ||
|| **GROUP_NAME**
[`string`](../data-types.md) | Group name ||
|| **ROLE**
[`string`](../data-types.md) | Role of the current user.

Possible values:
- `A` — owner
- `E` — moderator
- `K` — participant ||
|| **GROUP_IMAGE_ID**
[`string`](../data-types.md) | Group avatar identifier ||
|| **GROUP_IMAGE**
[`string`](../data-types.md) | URL of the group avatar ||
|| **IS_EXTRANET**
[`string`](../data-types.md) | Indicator of an extranet group.

This field is returned only for extranet groups:
- `Y` — extranet group ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sonet-group-get.md)
- [{#T}](./socialnetwork-api-workgroup-list.md)
- [{#T}](./socialnetwork-api-workgroup-get.md)
- [{#T}](./members/index.md)