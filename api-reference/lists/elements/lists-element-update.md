# Update List Element lists.element.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- success response is missing
- error response is missing
- links to pages that have not yet been created are not specified

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `lists.element.update` updates a list element. If the element is successfully updated, the response is `true`, otherwise *Exception*.

{% note warning %}

All fields of the element and their values must be passed in the request.

{% endnote %}

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **IBLOCK_TYPE_ID***
[`unknown`](../../data-types.md) | `id` of the information block:
- `lists` — list information block type
- `bitrix_processes` — processes information block type
- `lists_socnet` — group lists information block type ||
|| **IBLOCK_CODE/IBLOCK_ID***
[`unknown`](../../data-types.md) | Code or `id` of the information block ||
|| **ELEMENT_CODE/ELEMENT_ID***
[`unknown`](../../data-types.md) | Code or `id` of the element ||
|| **FIELDS**
[`unknown`](../../data-types.md) | Array of fields and values. For a field of type "File" `F`, pass the file in [Base64](../../files/how-to-update-files.md) format ||
|| **SOCNET_GROUP_ID***
[`unknown`](../../data-types.md) | `id` of the group. This parameter is required if the list is created for a group ||
|#

## Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists_socnet","IBLOCK_CODE":"rest_1","ELEMENT_CODE":"element_1","FIELDS":{"NAME":"Test element (Update)","PROPERTY_62":{"599":"Text string (Update)"},"PROPERTY_63":{"600":"73","601":"97","602":"17"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/lists.element.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists_socnet","IBLOCK_CODE":"rest_1","ELEMENT_CODE":"element_1","FIELDS":{"NAME":"Test element (Update)","PROPERTY_62":{"599":"Text string (Update)"},"PROPERTY_63":{"600":"73","601":"97","602":"17"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.element.update
    ```

- JS

    ```js
    var params = {
        'IBLOCK_TYPE_ID': 'lists_socnet',
        'IBLOCK_CODE': 'rest_1',
        'ELEMENT_CODE': 'element_1',
        'FIELDS': {
            'NAME': 'Test element (Update)',
            'PROPERTY_62': {
            '599': 'Text string (Update)'
            },
            'PROPERTY_63': {
            '600': '73',
            '601': '97',
            '602': '17'
            }
        }
    };
    BX24.callMethod(
        'lists.element.update',
        params,
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                alert("Success: " + result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'lists.element.update',
        [
            'IBLOCK_TYPE_ID' => 'lists_socnet',
            'IBLOCK_CODE' => 'rest_1',
            'ELEMENT_CODE' => 'element_1',
            'FIELDS' => [
                'NAME' => 'Test element (Update)',
                'PROPERTY_62' => [
                    '599' => 'Text string (Update)'
                ],
                'PROPERTY_63' => [
                    '600' => '73',
                    '601' => '97',
                    '602' => '17'
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

### How to Upload a File to a File Type Field (Drive)

1. Use the REST API of the disk module: `disk.folder.uploadfile` and `disk.storage.uploadfile`. In the response when uploading these files, you will receive `"ID": 290`.
2. Get the list of `ID`s of uploaded files.
3. Using the REST API of the lists module, add files to the required field. If the field already has attached files, you need to get the previous values from [lists.element.get](./lists-element-get.md) and pass them along with the new ones.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":"41","ELEMENT_CODE":"element1","FIELDS":{"NAME":"Test element 1","PROPERTY_121":{"4754":["50","n1582"]}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/lists.element.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists","IBLOCK_ID":"41","ELEMENT_CODE":"element1","FIELDS":{"NAME":"Test element 1","PROPERTY_121":{"4754":["50","n1582"]}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.element.update
    ```

- JS

    ```js
    var params = {
        'IBLOCK_TYPE_ID': 'lists',
        'IBLOCK_ID': '41',
        'ELEMENT_CODE': 'element1',
        'FIELDS': {
            'NAME': 'Test element 1',
            'PROPERTY_121': {'4754': ['50', 'n1582']} // or without id 'PROPERTY_121': {'n0': ['50', 'n1582']}
        }
    };
    BX24.callMethod(
        'lists.element.update',
        params,
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                alert("Success: " + result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'lists.element.update',
        [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => '41',
            'ELEMENT_CODE' => 'element1',
            'FIELDS' => [
                'NAME' => 'Test element 1',
                'PROPERTY_121' => ['4754' => ['50', 'n1582']]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

Values in the File (Drive) field without the prefix `"n"` are already attached files (`attachedId`), while those with the prefix are your new files that have already been uploaded to the drive.

### How to Delete a File

Find out the ID of file values using the method [lists.element.get](./lists-element-get.md).

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"bitrix_processes","IBLOCK_ID":47}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/lists.element.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"bitrix_processes","IBLOCK_ID":47,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.element.get
    ```

- JS

    ```js
    BX.rest.callMethod(
        'lists.element.get', {IBLOCK_TYPE_ID: 'bitrix_processes', IBLOCK_ID: 47}
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'lists.element.get',
        [
            'IBLOCK_TYPE_ID' => 'bitrix_processes',
            'IBLOCK_ID' => 47
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

You will receive a response in the following format.

```json
"result": [
    {
        "ID": "480",
        "IBLOCK_ID": "47",
        "NAME": "1",
        "IBLOCK_SECTION_ID": null,
        "CREATED_BY": "1",
        "BP_PUBLISHED": "Y",
        "CODE": "",
        "PROPERTY_133": {
            "2857": "375",
            "2858": "376"
        }
    }
],
```

Here, `PROPERTY_133` is a multiple field of type File. It represents an object where the key is the `ID` of the property value that you will need for deletion, and the value is the `ID` of the file.

To delete a property value, pass a field with the suffix `_DEL` to the method `lists.element.update`. Specify the list of values to be deleted. Use the `ID` of the property value as the key, and `"Y"` as the value.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"bitrix_processes","IBLOCK_ID":47,"ELEMENT_ID":480,"FIELDS":{"NAME":"1","PROPERTY_133_DEL":{"2857":"Y"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/lists.element.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"bitrix_processes","IBLOCK_ID":47,"ELEMENT_ID":480,"FIELDS":{"NAME":"1","PROPERTY_133_DEL":{"2857":"Y"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.element.update
    ```

- JS

    ```js
    BX.rest.callMethod(
        'lists.element.update', {
        IBLOCK_TYPE_ID: 'bitrix_processes',
        IBLOCK_ID: 47,
        ELEMENT_ID: 480,
        FIELDS: { NAME: '1', PROPERTY_133_DEL: {"2857": "Y"} }
    }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'lists.element.update',
        [
            'IBLOCK_TYPE_ID' => 'bitrix_processes',
            'IBLOCK_ID' => 47,
            'ELEMENT_ID' => 480,
            'FIELDS' => [
                'NAME' => '1',
                'PROPERTY_133_DEL' => ["2857" => "Y"]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}