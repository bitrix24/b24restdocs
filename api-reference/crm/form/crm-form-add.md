# Create a CRM Form crm.form.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit CRM forms

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `crm.form.add` method creates a CRM form with builder settings.

{% note warning "" %}

REST cannot add fields with choice options — lists, checkboxes, radio buttons — or products and booking to a form. The design theme cannot be changed either.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Form settings, [detailed description](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Form name, up to 50 characters ||
|| **crm***
[`object`](../../data-types.md) | CRM settings. The `entityScheme` field is required — the CRM scheme, that is, which objects a form submission creates. Scheme identifiers are returned by the [crm.form.configuration.get](./setup/crm-form-configuration-get.md) method in the `entitySchemes` key.

The optional fields of the section — `duplicateMode`, `dealCategoryId`, `smartProcessCategoryId`, `dealDuplicateControlEnabled`, `smartProcessDuplicateControlEnabled` — are described in the [crm section structure](./data-types.md#crm) ||
|| **active**
[`boolean`](../../data-types.md) | Form activity.

Possible values:

- `true` — the form works immediately after creation
- `false` — the form is created deactivated

Default: `true`.

An active form counts toward the plan's active forms limit ||
|| **fields**
[`array`](../../data-types.md) | Fields the client fills in, such as name and phone, in display order. This is the `fields` key inside the method's `fields` parameter — the names are the same.

Field codes must belong to the objects of the `crm.entityScheme` scheme and must not repeat, [detailed description](#form-fields) ||
|| **languageId**
[`string`](../../data-types.md) | Form language. The list of languages is returned by the [crm.form.configuration.get](./setup/crm-form-configuration-get.md) method ||
|| **templateId**
[`string`](../../data-types.md) | Form scenario. The list of scenarios is returned by the [crm.form.scenario.list](./setup/crm-form-scenario-list.md) method ||
|| **appearance**
[`object`](../../data-types.md) | Title, description, and button. The `title`, `description`, `buttonCaption`, `buttonTextColor`, `buttonBackgroundColor`, `copyrightRemoved` fields are described in the [appearance section structure](./data-types.md#appearance) ||
|| **responsible**
[`object`](../../data-types.md) | Queue of responsible persons. The `userIds`, `checkWorkTime` fields are described in the [responsible section structure](./data-types.md#responsible) ||
|| **agreements**
[`object`](../../data-types.md) | Consents. The `enabled`, `items` fields are described in the [agreements section structure](./data-types.md#agreements) ||
|| **submission**
[`object`](../../data-types.md) | Messages and redirect after submission. The `success`, `failure`, `redirectDelay`, `refill` fields are described in the [submission section structure](./data-types.md#submission) ||
|| **dependencies**
[`array`](../../data-types.md) | Dependent field rules. An element is a group of rules with the `logic` and `rules` fields; each rule has a `condition` and an `action`. See the [dependencies element structure](./data-types.md#dependencies) ||
|| **presetFields**
[`array`](../../data-types.md) | CRM field values that the form fills in automatically. The element with the `entityName`, `fieldName`, `value` fields is described in the [presetFields element structure](./data-types.md#presetFields) ||
|| **payment**
[`object`](../../data-types.md) | Payment after the form is submitted. The `enabled`, `disabledPaySystemsId` fields are described in the [payment section structure](./data-types.md#payment) ||
|| **callback**
[`object`](../../data-types.md) | Callback after the form is submitted. The `enabled`, `from`, `text` fields are described in the [callback section structure](./data-types.md#callback) ||
|| **analytics**
[`object`](../../data-types.md) | Web analytics counters. The `googleAnalyticsId`, `googleAnalyticsPageViewEnabled`, `yandexMetricaId` fields are described in the [analytics section structure](./data-types.md#analytics) ||
|| **spamProtection**
[`object`](../../data-types.md) | Spam protection: the `enabled` field of the `boolean` type ||
|| **integrations**
[`object`](../../data-types.md) | Messenger form and links to advertising services. The `whatsAppEnabled`, `ads` fields are described in the [integrations section structure](./data-types.md#integrations) ||
|#

#### Element fields.fields {#form-fields}

One array element is one form field. The field order is set by the order of the elements; there is no need to pass the `sort` field.

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Field code, for example `LEAD_NAME`. CRM field codes are returned by the [crm.form.entityfield.list](./setup/crm-form-entityfield-list.md) method. The field must belong to the objects of the `crm.entityScheme` scheme. A code can be specified in a form only once ||
|| **type**
[`string`](../../data-types.md) | Field type, for example `string`, `phone`, `email`. The list of types is returned by the [crm.form.configuration.get](./setup/crm-form-configuration-get.md) method in the `fieldTypes` key.

REST cannot yet add fields with choice options or products: `list`, `checkbox`, `radio`, `product`, `booking`. Such fields created in the interface are retained on write as long as their type is not changed ||
|| **label**
[`string`](../../data-types.md) | Field label ||
|| **required**
[`boolean`](../../data-types.md) | Whether the field is required.

Possible values:

- `true` — the field is required
- `false` — the field is optional ||
|| **multiple**
[`boolean`](../../data-types.md) | Whether multiple values can be entered.

Possible values:

- `true` — multiple values
- `false` — a single value ||
|| **placeholder**
[`string`](../../data-types.md) | Hint inside an empty field ||
|| **valueType**
[`string`](../../data-types.md) | Value type for contact fields, for example a work or mobile phone ||
|| **value**
[`string`](../../data-types.md) | Default value ||
|#

The `id`, `code`, `system`, `createdTime`, `updatedTime`, `activeChangedById`, `activeChangedTime`, `booking`, `appearance.backgroundImageId`, and `fields[].sort` fields are read-only and cannot be passed.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.form.add`

{% endnote %}

The example creates a deactivated form that creates a lead from a name and phone number.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Consultation Request","active":false,"crm":{"entityScheme":1},"fields":[{"name":"LEAD_NAME","type":"string","label":"Name","required":true},{"name":"LEAD_PHONE","type":"phone","label":"Phone","required":true}]}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.form.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Consultation Request","active":false,"crm":{"entityScheme":1},"fields":[{"name":"LEAD_NAME","type":"string","label":"Name","required":true},{"name":"LEAD_PHONE","type":"phone","label":"Phone","required":true}]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.form.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FormAddResult = {
      id: number
    }

    try {
      const response = await $b24.actions.v3.call.make<FormAddResult>({
        method: 'crm.form.add',
        params: {
          fields: {
            name: 'Consultation Request',
            active: false,
            crm: {
              entityScheme: 1,
            },
            fields: [
              {
                name: 'LEAD_NAME',
                type: 'string',
                label: 'Name',
                required: true,
              },
              {
                name: 'LEAD_PHONE',
                type: 'phone',
                label: 'Phone',
                required: true,
              },
            ],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.id)
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
      async function addForm() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'crm.form.add',
            params: {
              fields: {
                name: 'Consultation Request',
                active: false,
                crm: {
                  entityScheme: 1,
                },
                fields: [
                  {
                    name: 'LEAD_NAME',
                    type: 'string',
                    label: 'Name',
                    required: true,
                  },
                  {
                    name: 'LEAD_PHONE',
                    type: 'phone',
                    label: 'Phone',
                    required: true,
                  },
                ],
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
          console.info(result.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addForm)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    # client = Client(token, prefer_version=3)

    try:
        bitrix_response = client.crm.form.add(
            fields={
                "name": "Consultation Request",
                "active": False,
                "crm": {
                    "entityScheme": 1,
                },
                "fields": [
                    {
                        "name": "LEAD_NAME",
                        "type": "string",
                        "label": "Name",
                        "required": True,
                    },
                    {
                        "name": "LEAD_PHONE",
                        "type": "phone",
                        "label": "Phone",
                        "required": True,
                    },
                ],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.form.add',
                [
                    'fields' => [
                        'name' => 'Consultation Request',
                        'active' => false,
                        'crm' => [
                            'entityScheme' => 1,
                        ],
                        'fields' => [
                            [
                                'name' => 'LEAD_NAME',
                                'type' => 'string',
                                'label' => 'Name',
                                'required' => true,
                            ],
                            [
                                'name' => 'LEAD_PHONE',
                                'type' => 'phone',
                                'label' => 'Phone',
                                'required' => true,
                            ],
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'crm.form.add',
        {
          fields: {
            name: 'Consultation Request',
            active: false,
            crm: {
              entityScheme: 1,
            },
            fields: [
              {
                name: 'LEAD_NAME',
                type: 'string',
                label: 'Name',
                required: true,
              },
              {
                name: 'LEAD_PHONE',
                type: 'phone',
                label: 'Phone',
                required: true,
              },
            ],
          },
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
        'crm.form.add',
        [
            'fields' => [
                'name' => 'Consultation Request',
                'active' => false,
                'crm' => [
                    'entityScheme' => 1,
                ],
                'fields' => [
                    [
                        'name' => 'LEAD_NAME',
                        'type' => 'string',
                        'label' => 'Name',
                        'required' => true,
                    ],
                    [
                        'name' => 'LEAD_PHONE',
                        'type' => 'phone',
                        'label' => 'Phone',
                        'required' => true,
                    ],
                ],
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client's base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "crm.form.add", b24.Params{
    	"fields": map[string]any{
    		"name": "Consultation Request",
    		"active": false,
    		"crm": map[string]any{
    			"entityScheme": 1,
    		},
    		"fields": []any{
    			map[string]any{
    				"name": "LEAD_NAME",
    				"type": "string",
    				"label": "Name",
    				"required": true,
    			},
    			map[string]any{
    				"name": "LEAD_PHONE",
    				"type": "phone",
    				"label": "Phone",
    				"required": true,
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.form.add: %w", err)
    }

    var data struct {
    	ID int `json:"id"`
    }
    if err := json.Unmarshal(res.Result, &data); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(data.ID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "id": 47
    },
    "time": {
        "start": 1792593840,
        "finish": 1792593842.660519,
        "duration": 2.6605188846588135,
        "processing": 2,
        "date_start": "2026-10-21T17:44:00+02:00",
        "date_finish": "2026-10-21T17:44:02+02:00",
        "operating_reset_at": 1792594440,
        "operating": 2.563063859939575
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the response data ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the created form ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
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
                "message": "Name of the CRM form is required",
                "field": "name"
            },
            {
                "message": "CRM scheme of the form can not be used",
                "field": "crm.entityScheme"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

The method returns all configuration violations at once. The `field` field contains the path to the request value, for example `crm.entityScheme` or `fields[1].name`. Configuration validation messages are returned in English.

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the form settings in `fields` ||
|| `name` | Name of the CRM form is required | Pass the form name ||
|| `name` | Name of the CRM form is longer than 50 characters | Shorten the name to 50 characters ||
|| `crm.entityScheme` | CRM scheme of the form is required | Pass the CRM scheme ||
|| `crm.entityScheme` | CRM scheme of the form can not be used | Take the scheme identifier from the [crm.form.configuration.get](./setup/crm-form-configuration-get.md) response ||
|| `fields[N].type` | A field of type `list` can not be created through this contract: the variants it offers are not part of it | Do not add fields of the `list`, `checkbox`, `radio`, `product`, `booking` types via REST ||
|| `fields[N].name` | Form field `LEAD_NAME` is specified more than once | Specify each field code only once ||
|| `fields[N].name` | CRM field `#NAME#` is not part of the scheme of the form | Use the fields of the selected scheme's objects from the [crm.form.entityfield.list](./setup/crm-form-entityfield-list.md) response ||
|| `booking` | `booking` is published for reading only | Do not pass read-only fields ||
|| `payment.enabled` | Payment is not available on this portal | Payment is not available on this Bitrix24 ||
|| `callback.enabled` | Callback is not available on this portal | Callback is not available on this Bitrix24 ||
|| `integrations.whatsAppEnabled` | Messenger forms are not available on this portal | Messenger forms are not available on this Bitrix24 ||
|| `callback.from` | This number can not be called from on this portal | Specify a telephony number of this Bitrix24 ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Unknown field `sort` found in entity `FormDto` | Pass only the fields listed in the [CRM Form Structure](./data-types.md#crm_form) section ||
|#

#### Form Partially Created Error

Error Code: `CRM_FORM_PARTIALLY_CREATED`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | CRM form created but not all data fields could be saved. Check the fields and update the form by ID | The response contains the `id` key of the created form. Do not create the form again: fix the fields listed in `validation` and update the form using the [crm.form.update](./crm-form-update.md) method ||
|#

#### Plan Restrictions

Error Code: `CRM_FORM_WRITE_RESTRICTED`

HTTP status: **402**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | You cannot modify CRM forms on your plan | Change the Bitrix24 plan ||
|#

Error Code: `CRM_FORM_ACTIVATION_LIMIT_REACHED`

HTTP status: **402**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Could not enable the CRM form because the plan limit of active forms is reached. Disable another form and retry the request | Create the form with `active: false` or deactivate another form using the [crm.form.deactivate](./crm-form-deactivate.md) method ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has permission to edit CRM forms ||
|#

Error Code: `CRM_FORM_PHONE_VERIFICATION_REQUIRED`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | You have to confirm your Bitrix24 phone number to use CRM forms | Verify the phone number in Bitrix24 and repeat the request ||
|#

Error Code: `CRM_FORM_MODULE_UNAVAILABLE`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | The "#MODULE#" module is required | Install the module named in the message ||
|#

#### Post-Save Error

Error Code: `CRM_FORM_SIDE_EFFECT_FAILED`

HTTP status: **500**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Operation failed but CRM form data was modified. Check the form before trying again | Retrieve the form using the [crm.form.get](./crm-form-get.md) method and check its state ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-form-validate.md)
- [{#T}](./crm-form-update.md)
- [{#T}](./data-types.md)
- [{#T}](./setup/crm-form-configuration-get.md)
- [{#T}](./crm-form-activate.md)
- [{#T}](./setup/crm-form-entityfield-list.md)
- [{#T}](./placement/crm-form-embedding-get.md)
- [{#T}](./index.md)
