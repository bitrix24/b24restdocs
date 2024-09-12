# Create a New Document Based on a Template documentgenerator.document.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- adjustments needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`documentgenerator`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `documentgenerator.document.add` creates a new document based on a template.

#|
|| **Parameter** | **Description** ||
|| **templateId** | Template ID. ||
|| **value** | External identifier. The value parameter is needed only for the application interface, as an identifier for the external source. This is a string parameter, and you can pass a complete external code into it. For example, `"PARTNER_APP_10_BILL_133”`. ||
|| **values** | A set of field values for the document. In a simple case, this is a one-dimensional array where the key is the field name and the value is the string to be inserted into the document. ||
|| **stampsEnabled** | 1 (enable), 0 (disable) stamps and signatures. ||
|| **fields** | Description of the document fields. This parameter is an array where the key is the field name and the value is the description. Codes for simple field types: 
- TYPE — field type; 
- FORMAT — format; 
- PROVIDER — provider; 
- TITLE — title; 
- IMAGE — image; 
- STAMP — stamp/signature; 
- DATE — date/time; 
- NAME — name; 
- PHONE — phone number. ||
|#

## Examples

You can view [examples of document generation](./examples/index.md) here.

## Response on Success

In case of successful execution, the result will return a structure similar to the method [documentgenerator.document.get()](./document-generator-document-get.md) for the new document.

## Why is there no link to the PDF in the result of document.add

The conversion to **pdf** is an asynchronous operation. At the time the document generation is completed, the pdf file is not yet available.

If a pdf is urgently needed for the document, the only option right now is to make a repeated request to `documentgenerator.document.get` after 20-30 seconds to retrieve the link to the pdf. If it does not appear there, try again.