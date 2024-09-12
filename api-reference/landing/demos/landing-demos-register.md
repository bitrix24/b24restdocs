# Register a Template in the Site Creation Wizard landing.demos.register

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed to meet writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent
- links to pages that have not yet been created (localization of blocks) are not specified

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.demos.register` registers a template in the site and page creation wizard. It returns an array of identifiers for the created templates. The execution of the method is interrupted when an error occurs in the array of elements, and an error description is returned.

{% note info %}

To distribute the created site, simply export it to a file from the source account and distribute its application by calling this method during installation.

{% endnote %}

## Parameters

#|
|| **Parameter** | **Description** ||
|| **data**
[`unknown`](../../data-types.md) | The result of the method [landing.site.fullExport](../site/landing-site-full-export.md) as is. ||
|| **params**
[`unknown`](../../data-types.md) | May contain the following keys (**only for on-premise versions**):
- **site_template_id** - binding the block to a specific site template (main module). ||
|#

## Localization

For clarifications on template localizations, please see [here](./localization.md). When localization is required, uncomment the keys **lang** and **lang_original**. The principle used here is similar to [localization of blocks](.). Please note that localization applies only to main phrases: page titles, descriptions. Do not overload this array with unnecessary information.

## Examples

Note that the example uses the result of the method [landing.site.fullExport](../site/landing-site-full-export.md).

```js
BX24.callMethod(
    'landing.site.fullExport',
    {
        id: 326,
        params: {
            edit_mode: 'Y',
            code: 'myfirstsite',//symbolic code of site
            name: 'Auto Repair Shop Website',// name of the site (page)
            description: 'Website for your auto service. Everything you need under the hood.',// description of the site
            preview_url: 'http://sample.landing.mycompany.com/',// preview URL
            preview: 'http://site.com/preview.jpg',// main preview image for the template list (recommended 280x115)
            preview2x: 'http://site.com/preview.jpg',// enlarged preview image (recommended 560x230)
            preview3x: 'http://site.com/preview.jpg',// retina-sized preview image (recommended 845x345)
        }
    },
    function(result)
    {
        if(result.error())
        {
            console.error(result.error());
        }
        else
        {
            var data = result.data();
            console.info(data);
            BX24.callMethod(
                'landing.demos.register',
                {
                    data: data,
                    params: {
                        site_template_id: '',// pass the template value if you are registering for your template (only on-premise!)
                        // localization array and original language
                        /*lang: {
                            en: {
                                'Phrase 1': 'Translate en 1',
                                'Phrase 2': 'Translate en 2'
                            },
                            de: {
                                'Phrase 1': 'Translate de 1',
                                'Phrase 2': 'Translate de 2'
                            }
                        },
                        lang_original: 'ru'*/
                    }
                },
                function(result)
                {
                    if(result.error())
                    {
                        console.error(result.error());
                    }
                    else
                    {
                        console.info(result.data());
                    }
                }
            );
        }
    }
);
```

{% include [Example Note](../../../_includes/examples.md) %}