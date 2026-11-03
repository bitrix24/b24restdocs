# Make a Linked Contact Primary v 3.0 crm.[type].setprimarycontact

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit items of this CRM object type

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.setprimarycontact` method makes one of the contacts already linked to a CRM item the primary contact. The object type is set by the `<type>` segment of the method name.

The primary contact is the one whose ID the item returns in the `contactId` field. All linked contacts are listed in the `contactsId` field. The method does not link a new contact: first add it with the [crm.&lt;type&gt;.addfieldvalue](./crm-entity-add-field-value-rest-v3.md) method using the `contactsId` field.

## Method Name for the Object Type

#|
|| **Object type** | **Method** | **Fields** ||
|| Deal | `crm.deal.setprimarycontact` | [Deal fields](./fields-deal.md) ||
|| Lead | `crm.lead.setprimarycontact` | [Lead fields](./fields-lead.md) ||
|| Estimate | `crm.quote.setprimarycontact` | [Estimate fields](./fields-quote.md) ||
|| Invoice | `crm.smartinvoice.setprimarycontact` | [Invoice fields](./fields-smart-invoice.md) ||
|| CRM document | `crm.smartdocument.setprimarycontact` | — ||
|| B2E document | `crm.smartb2edocument.setprimarycontact` | — ||
|| SPA | `crm.smartprocess<entityTypeId>.setprimarycontact`, for example, `crm.smartprocess1038.setprimarycontact` | [SPA fields](./fields-smart-process.md) ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field.

The method name is written in lowercase: `crm.smartinvoice.setprimarycontact`, not `crm.smartInvoice.setprimarycontact`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

An SPA has this method only if client linking is enabled for its type. Otherwise, the call returns the ["Method Not Found"](#method-not-found) error with status 404. Contacts, companies, and my companies do not have this method.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Item ID. Item IDs can be retrieved with the [crm.&lt;type&gt;.list](./crm-entity-list-rest-v3.md) method ||
|| **contactId***
[`integer`](../../../data-types.md) | ID of the contact to make primary. The contact must be in the `contactsId` field of the item. Linked contacts are returned by the [crm.&lt;type&gt;.get](./crm-entity-get-rest-v3.md) method ||
|#

If the contact is already primary, the method returns `true` and changes nothing. The method does not change the order of contacts in `contactsId`.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.setprimarycontact`

{% endnote %}

The examples make contact `3215` the primary contact of an SPA item with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":27,"contactId":3215}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.setprimarycontact
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":27,"contactId":3215,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.setprimarycontact
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type SetPrimaryContactResult = {
      result: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<SetPrimaryContactResult>(
        'crm.smartprocess1038.setprimarycontact',
        {
          id: 27,
          contactId: 3215,
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Primary contact set:', result.result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function setPrimaryContact() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.setprimarycontact',
            {
              id: 27,
              contactId: 3215,
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Primary contact set:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setPrimaryContact)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.setprimarycontact"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 27,
                "contactId": 3215,
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"code: {error.code}",
            f"message: {error.error.message}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.smartprocess1038.setprimarycontact',
                [
                    'id' => 27,
                    'contactId' => 3215,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'crm.smartprocess1038.setprimarycontact',
        {
            id: 27,
            contactId: 3215,
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.smartprocess1038.setprimarycontact',
        [
            'id' => 27,
            'contactId' => 3215,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.setprimarycontact", b24.Params{
    	"id":        27,
    	"contactId": 3215,
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.setprimarycontact: %w", err)
    }

    var updated struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &updated); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(updated.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "result": true
    },
    "time": {
        "start": 1793706737,
        "finish": 1793706737.554163,
        "duration": 0.5541629791259766,
        "processing": 0,
        "date_start": "2026-11-03T14:52:17+03:00",
        "date_finish": "2026-11-03T14:52:17+03:00",
        "operating_reset_at": 1793707320,
        "operating": 0.17444896697998047
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data ||
|| **result.result**
[`boolean`](../../../data-types.md) | `true` if the contact is primary. The primary contact ID can be retrieved with the [crm.&lt;type&gt;.get](./crm-entity-get-rest-v3.md) method in the `contactId` field ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "The contact is not bound to the item.",
                "field": "CONTACT_NOT_BOUND"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Item Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID #ID# was not found | Check the item ID. This object type has no item with this `id`, or the item has been deleted ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass the item `id` ||
|| `contactId` | Required field `contactId` is missing | Pass the contact ID ||
|| `contactId` | Field value must be 1 or greater | Pass a positive ID ||
|| `contactId` | This request requires data type `int` in field `contactId` | Pass the ID as a number ||
|| `CONTACT_NOT_BOUND` | The contact is not bound to the item | The contact is not linked to the item. Link it with the [crm.&lt;type&gt;.addfieldvalue](./crm-entity-add-field-value-rest-v3.md) method using the `contactsId` field ||
|| `CONTACT_NOT_FOUND` | The contact does not exist | There is no contact with this ID. Check `contactId` ||
|#

In the `CONTACT_NOT_BOUND` and `CONTACT_NOT_FOUND` errors, the `field` key contains the error code instead of a field name.

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.setprimarycontact`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-update-rest-v3.md)
- [{#T}](./crm-entity-get-rest-v3.md)
- [{#T}](./crm-entity-add-field-value-rest-v3.md)
- [{#T}](./crm-entity-delete-field-value-rest-v3.md)
