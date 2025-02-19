# Add Inventory Document catalog.document.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```http
catalog.document.add(fields)
```

This method adds an [`inventory document`](../enum/catalog-enum-get-store-document-types.md).
If the operation is successful, it returns the `id` of the added document.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields**
[`array`](../../data-types.md)| Parameters of the document being added. The fields correspond to the available list of fields [`fields`](catalog-document-get-fields.md). ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"docType":"S","contractorId":"1","responsibleId":"1","dateModify":"2000-01-01T00:00:00+01:00","dateCreate":"2000-01-01T00:00:00+01:00","createdBy":"1","modifiedBy":"1","currency":"USD","status":"S","dateStatus":"2000-01-01T00:00:00+01:00","dateDocument":"2000-01-01T00:00:00+01:00","statusBy":"1","total":"100","commentary":"first document.","title":"New Document"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"docType":"S","contractorId":"1","responsibleId":"1","dateModify":"2000-01-01T00:00:00+01:00","dateCreate":"2000-01-01T00:00:00+01:00","createdBy":"1","modifiedBy":"1","currency":"USD","status":"S","dateStatus":"2000-01-01T00:00:00+01:00","dateDocument":"2000-01-01T00:00:00+01:00","statusBy":"1","total":"100","commentary":"first document.","title":"New Document"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.add
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.document.add',
        {
            'fields': {
                'docType': 'S',
                'contractorId': '1',
                'responsibleId': '1',
                'dateModify': '2000-01-01T00:00:00+01:00',
                'dateCreate': '2000-01-01T00:00:00+01:00',
                'createdBy': '1',
                'modifiedBy': '1',
                'currency': 'USD',
                'status': 'S',
                'dateStatus': '2000-01-01T00:00:00+01:00',
                'dateDocument': '2000-01-01T00:00:00+01:00',
                'statusBy': '1',
                'total': '100',
                'commentary': 'first document.',
                'title': 'New Document',
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.document.add',
        [
            'fields' => [
                'docType' => 'S',
                'contractorId' => '1',
                'responsibleId' => '1',
                'dateModify' => '2000-01-01T00:00:00+01:00',
                'dateCreate' => '2000-01-01T00:00:00+01:00',
                'createdBy' => '1',
                'modifiedBy' => '1',
                'currency' => 'USD',
                'status' => 'S',
                'dateStatus' => '2000-01-01T00:00:00+01:00',
                'dateDocument' => '2000-01-01T00:00:00+01:00',
                'statusBy' => '1',
                'total' => '100',
                'commentary' => 'first document.',
                'title' => 'New Document',
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}