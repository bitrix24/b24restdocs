# Add Page or Folder landing.landing.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.landing.add` adds a page or folder to the specified site and returns the identifier of the created object. The new object is created as inactive (`ACTIVE = N`). Additional fields of the page, such as meta tags and the color theme, are set after creation with the [landing.landing.update](./landing-landing-update.md) method.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal landing parameter: the type of sites the method works with. It is not related to the REST scope `landing`.

For Knowledge Base pages, pass `KNOWLEDGE`; for group knowledge base pages, pass `GROUP`. The value is case-insensitive. Without `scope`, the method cannot find a Knowledge Base site and returns the `SITE_NOT_FOUND` error. For sites of the `PAGE`, `STORE`, `SMN`, and `VIBE` types, the parameter is not needed [(detailed description)](../../types.md) ||
|| **fields***
[`object`](../../../data-types.md) | Set of fields for the new page or folder [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **TITLE***
[`string`](../../../data-types.md) | Title of the page ||
|| **SITE_ID***
[`integer`](../../../data-types.md) | Identifier of the site where the page is created.

The site identifier can be obtained using the method [landing.site.getList](../../site/landing-site-get-list.md) or from the result of the method [landing.site.add](../../site/landing-site-add.md) ||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the page, part of its URL. The code must not contain `/` or [forbidden characters](#errors): quotes, `$`, brackets, and others. The code cannot be passed in the format `<letters, digits or _>_<number>_<number>`, for example `code_12_34`.

If the field is not provided or a string consisting only of spaces is passed, the code will be generated from `TITLE`. If the transliterated code turns out to be empty, a string of 12 characters will be generated.

An empty string `''` without the `FOLDER` parameter cannot be passed — the method will return the `CANT_BE_EMPTY` error. If the `FOLDER` parameter is passed with any value, an empty string is allowed, and the code will be generated from `TITLE`.

If the code is already taken within the site or folder, a suffix of the form `_<4 random characters>` will be added to it ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | Arbitrary description of the page ||
|| **XML_ID**
[`string`](../../../data-types.md) | External identifier of the page ||
|| **SITEMAP**
[`string`](../../../data-types.md) | Flag to include the page in the sitemap. Supported values are `Y` and `N`, default is `N` ||
|| **FOLDER**
[`string`](../../../data-types.md) | Create a folder together with the page. Supported values are `Y` and `N`, default is `N`.

With `Y` and without `FOLDER_ID`, Bitrix24 creates a folder with the title and code of the page in the same call and makes the new page its index page. The response contains the page identifier; the page has `FOLDER = N`, and `FOLDER_ID` contains the identifier of the new folder. If you pass `FOLDER_ID` together with `Y`, the new page has `FOLDER = Y`, the flag of a folder page [(detailed description)](../fields.md) ||
|| **FOLDER_ID**
[`integer`](../../../data-types.md) | Identifier of the folder where the page should be created. The folder must belong to the same site as `SITE_ID`.

The folder identifier can be obtained using the method [landing.site.getFolders](../../site/landing-site-get-folders.md) ||
|| **TPL_ID**
[`integer`](../../../data-types.md) | Identifier of the page view template.

The identifier of the view template can be obtained using the method [landing.template.getlist](../../template/landing-template-get-list.md) ||
|| **ADDITIONAL_FIELDS**
[`object`](../../../data-types.md) | Additional fields of the page. They are set after creation with the [landing.landing.update](./landing-landing-update.md) method: pass the identifier of the new page in `lid`, and the field codes and values from the article [Additional Page Fields](../additional-fields.md) in `fields.ADDITIONAL_FIELDS` ||
|| **BLOCK_ID**
[`integer`](../../../data-types.md) | Used together with `MENU_CODE` to add a link to the menu of the block with the specified identifier after the page is created ||
|| **MENU_CODE**
[`string`](../../../data-types.md) | Used together with `BLOCK_ID` to specify the menu code in the block where the link to the created page should be added ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "fields": {
          "TITLE": "Spring Promotion",
          "SITE_ID": 292,
          "CODE": "spring-sale",
          "DESCRIPTION": "Page for seasonal promotion"
        }
      }' \
      "https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/landing.landing.add.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "fields": {
          "TITLE": "Spring Promotion",
          "SITE_ID": 292,
          "CODE": "spring-sale",
          "DESCRIPTION": "Page for seasonal promotion"
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put_your_bitrix24_address**/rest/landing.landing.add.json"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'landing.landing.add',
        params: {
          fields: {
            TITLE: 'Spring Promotion',
            SITE_ID: 292,
            CODE: 'spring-sale',
            DESCRIPTION: 'Page for seasonal promotion',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created landing page ID:', result)
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
      async function addLandingPage() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.landing.add',
            params: {
              fields: {
                TITLE: 'Spring Promotion',
                SITE_ID: 292,
                CODE: 'spring-sale',
                DESCRIPTION: 'Page for seasonal promotion',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Created landing page ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addLandingPage)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.landing.landing.add(
            fields={
                "TITLE": "Spring Promotion",
                "SITE_ID": 292,
                "CODE": "spring-sale",
                "DESCRIPTION": "Page for seasonal promotion",
            },
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
                'landing.landing.add',
                [
                    'fields' => [
                        'TITLE' => 'Spring Promotion',
                        'SITE_ID' => 292,
                        'CODE' => 'spring-sale',
                        'DESCRIPTION' => 'Page for seasonal promotion',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding landing page: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.add',
        {
            fields: {
                TITLE: 'Spring Promotion',
                SITE_ID: 292,
                CODE: 'spring-sale',
                DESCRIPTION: 'Page for seasonal promotion',
            }
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
        'landing.landing.add',
        [
            'fields' => [
                'TITLE' => 'Spring Promotion',
                'SITE_ID' => 292,
                'CODE' => 'spring-sale',
                'DESCRIPTION' => 'Page for seasonal promotion',
            ],
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
    res, err := client.Core().Call(ctx, "landing.landing.add", b24.Params{
    	"fields": b24.Params{
    		"TITLE":       "Spring Promotion",
    		"SITE_ID":     292,
    		"CODE":        "spring-sale",
    		"DESCRIPTION": "Page for seasonal promotion",
    	},
    })
    if err != nil {
    	return fmt.Errorf("landing.landing.add: %w", err)
    }

    var newID b24.ID
    if err := json.Unmarshal(res.Result, &newID); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("id:", newID)
    ```

{% endlist %}

{% note warning %}

If you want the new page to immediately appear in the block menu, pass both service parameters: `BLOCK_ID` and `MENU_CODE` in `fields`

{% endnote %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 2227,
    "time": {
        "start": 1773694924,
        "finish": 1773694924.307754,
        "duration": 0.3077540397644043,
        "processing": 0,
        "date_start": "2026-03-17T00:02:04+01:00",
        "date_finish": "2026-03-17T00:02:04+01:00",
        "operating_reset_at": 1773695524,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the created page or folder ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "SLASH_IS_NOT_ALLOWED",
    "error_description": "Slash is not allowed in the landing address."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes {#errors}

#| 
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required top-level parameter `fields` is missing ||
|| `ACCESS_DENIED` | Access to create the page is denied: the user does not have "edit" permission for the specified site ||
|| `SITE_NOT_FOUND` | Site not found: a non-existent site identifier is passed in `fields.SITE_ID` ||
|| `FOLDER_NOT_FOUND` | Folder not found: a folder that does not belong to the specified site or does not exist is passed in `fields.FOLDER_ID` ||
|| `SLASH_IS_NOT_ALLOWED` | Slash is not allowed in the landing address: a character `/` is passed in `fields.CODE` ||
|| `CANT_BE_EMPTY` | The page address cannot be empty: an empty string `''` is passed in `fields.CODE`, and `fields.FOLDER` is not passed ||
|| `WRONG_CODE_CHARS` | The page address contains forbidden characters: `fields.CODE` contains single, double, or back quotes, `$`, `\`, `<`, `>`, parentheses or curly brackets, as well as control characters, for example, a line break ||
|| `WRONG_CODE_FORMAT` | Invalid page address: a value in the format `<letters, digits or _>_<number>_<number>` is passed in `fields.CODE`, for example `code_12_34` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-add-by-template.md)
- [{#T}](./landing-landing-copy.md)
- [{#T}](./landing-landing-delete.md)
- [{#T}](./landing-landing-get-additional-fields.md)
- [{#T}](./landing-landing-get-list.md)
- [{#T}](./landing-landing-publication.md)
- [{#T}](./landing-landing-update.md)