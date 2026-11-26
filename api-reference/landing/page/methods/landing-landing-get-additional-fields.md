# Get Additional Fields of the Page landing.landing.getadditionalfields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "view" access permission for the site

The method `landing.landing.getadditionalfields` retrieves non-empty [additional fields](../additional-fields.md) of the page, such as meta tags and the color theme.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal landing parameter: the type of sites the method works with. It is not related to the REST scope `landing`.

For Knowledge Base pages, pass `KNOWLEDGE`; for group knowledge base pages, pass `GROUP`. The value is case-insensitive. Without `scope`, the method cannot find a Knowledge Base page and returns the `LANDING_NOT_EXIST` error. For sites of the `PAGE`, `STORE`, `SMN`, and `VIBE` types, the parameter is not needed [(detailed description)](../../types.md) ||
|| **lid***
[`integer`](../../../data-types.md) | Identifier of the page.

The page identifier can be obtained using the [landing.landing.getList](./landing-landing-get-list.md) method, as well as from the results of the [landing.landing.add](./landing-landing-add.md), [landing.landing.addByTemplate](./landing-landing-add-by-template.md), and [landing.landing.copy](./landing-landing-copy.md) methods ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 349
      }' \
      "https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/landing.landing.getadditionalfields.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 349,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put_your_bitrix24_address**/rest/landing.landing.getadditionalfields.json"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    // result is an empty array [] when the page has no filled fields, e.g. a Knowledge Base page without meta tags
    type AdditionalFieldsResult = Record<string, unknown> | []

    try {
      const response = await $b24.actions.v2.call.make<AdditionalFieldsResult>({
        method: 'landing.landing.getadditionalfields',
        params: {
          lid: 349,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Additional fields:', Object.keys(result).length, result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getAdditionalFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.landing.getadditionalfields',
            params: {
              lid: 349,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Additional fields:', Object.keys(result).length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getAdditionalFields)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.landing.landing.getadditionalfields(
            lid=349,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```
- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.landing.getadditionalfields',
                [
                    'lid' => 349,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting additional fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.getadditionalfields',
        {
            lid: 349
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'landing.landing.getadditionalfields',
        [
            'lid' => 349,
        ]
    );

    if (isset($result['error']))
    {
        echo 'Error: ' . $result['error_description'];
    }
    else
    {
        echo '<pre>';
        print_r($result['result']);
        echo '</pre>';
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "landing.landing.getadditionalfields", b24.Params{
    	"lid": 349,
    })
    if err != nil {
    	return fmt.Errorf("landing.landing.getadditionalfields: %w", err)
    }

    // For a page without filled fields, for example, a Knowledge Base page without meta tags, result is an empty array []
    if string(res.Result) == "[]" {
    	fmt.Println("no additional fields")
    	return nil
    }

    var item struct {
    	FontsCode     string `json:"FONTS_CODE"`
    	GacounterUse  string `json:"GACOUNTER_USE"`
    	MetamainUse   string `json:"METAMAIN_USE"`
    	MetamainTitle string `json:"METAMAIN_TITLE"`
    	MetaogTitle   string `json:"METAOG_TITLE"`
    	MetaogImage   string `json:"METAOG_IMAGE"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.FontsCode, item.GacounterUse)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

Below is a shortened example of a response. The actual set of fields depends on the page settings and the additional fields connected to it.

```json
{
    "result": {
        "FONTS_CODE": "<noscript><link rel=\"stylesheet\" href=\"https://fonts.googleapis.com/...\" data-font=\"g-font-russo-one\"></noscript>",
        "GACOUNTER_USE": "N",
        "METAMAIN_USE": "Y",
        "METAMAIN_TITLE": "Festival in New York. April 20-26, 2022. Buy tickets online",
        "METAOG_TITLE": "Festival in New York. April 20-26, 2022. Buy tickets online",
        "METAOG_IMAGE": "https://cdn.com/.../cover_1x.webp",
        "SETTINGS_PRICE_CODE": [
            "BASE"
        ],
        "SETTINGS_SHOW_PRICE_COUNT": 1,
        "THEMEFONTS_LINE_HEIGHT": "1.6",
        "VIEW_TYPE": "no",
        "YACOUNTER_USE": "N"
    },
    "time": {
        "start": 1773722096,
        "finish": 1773722096.682451,
        "duration": 0.6824510097503662,
        "processing": 0,
        "date_start": "2026-03-17T05:34:56+01:00",
        "date_finish": "2026-03-17T05:34:56+01:00",
        "operating_reset_at": 1773722696,
        "operating": 0.11843705177307129
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) \| [`array`](../../../data-types.md) | Additional fields of the page in the format `{"<FIELD_CODE>": "<VALUE>"}`. Empty fields are not included in the response: if there are no non-empty fields, the method returns an empty array `[]` [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Object result {#result}

The object key is the field code. The table lists frequently used fields; all codes and values are described in the article [Additional Fields of the Page](../additional-fields.md).

#|
|| **Name**
`type` | **Description** ||
|| **METAMAIN_USE**
[`string`](../../../data-types.md) | Whether custom meta tags of the page are enabled: `Y` or `N` ||
|| **METAMAIN_TITLE**
[`string`](../../../data-types.md) | Value of the `title` tag ||
|| **METAMAIN_DESCRIPTION**
[`string`](../../../data-types.md) | Value of the `description` tag ||
|| **METAOG_TITLE**
[`string`](../../../data-types.md) | Title of the page preview in social networks and messengers, Open Graph tag `og:title` ||
|| **METAOG_DESCRIPTION**
[`string`](../../../data-types.md) | Description of the page preview in social networks and messengers, Open Graph tag `og:description` ||
|| **METAOG_IMAGE**
[`string`](../../../data-types.md) | Image for Open Graph, tag `og:image`. If the field contains a file identifier, an application receives a link to the file, and a webhook receives the identifier itself ||
|| **METAROBOTS_INDEX**
[`string`](../../../data-types.md) | Whether indexing of the page is allowed: `Y` or `N` ||
|| **THEME_CODE**
[`string`](../../../data-types.md) | The color theme code of the page from the section [Page Color Themes](../color-themes.md) ||
|| **VIEW_TYPE**
[`string`](../../../data-types.md) | Type of view of the page: `no`, `ltr`, `all`, `mobile`, or `adaptive` ||
|| **SETTINGS_\***
[`string`](../../../data-types.md) \| [`integer`](../../../data-types.md) \| [`array`](../../../data-types.md) | Catalog and store settings. If the catalog module is installed in Bitrix24, these fields are returned for pages of sites and stores with default values, even if they were not set. Fields with an empty default value are not returned. Knowledge Base pages have no `SETTINGS_*` fields. The type depends on the field, for example, `SETTINGS_PRICE_CODE` is an array of price type codes, and `SETTINGS_SHOW_PRICE_COUNT` is a number ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "LANDING_NOT_EXIST",
    "error_description": "Landing not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `lid` is missing ||
|| `LANDING_NOT_EXIST` | Page not found: the `lid` parameter contains the identifier of a non-existent or inaccessible page ||
|| `ACCESS_DENIED` | Insufficient rights to call the method ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-add.md)
- [{#T}](./landing-landing-add-by-template.md)
- [{#T}](./landing-landing-copy.md)
- [{#T}](./landing-landing-update.md)
- [{#T}](./landing-landing-get-list.md)
- [{#T}](./landing-landing-get-preview.md)
- [{#T}](./landing-landing-get-public-url.md)