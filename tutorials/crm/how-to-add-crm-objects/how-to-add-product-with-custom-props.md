# How to Add a Product with Custom Field Values

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the CRM section

Example of filling various properties when adding a product.

To run the example, create a folder **/pictures** next to the executable file of the example and fill it with images named "1.jpg" — "6.jpg". Also, at the beginning of the example, you need to update the variable values in the example to your own:

- `propertyIDSelect` — identifier of the single-select property
- `propertySelectValueID` — identifier of the value of the single-select property
- `propertyIDMultiSelect` — identifier of the multi-select property
- `propertyMultiSelectValueID` — identifiers of the values of the multi-select property
- `propertyIDFile` — identifier of the single file property
- `propertyIDMultiFile` — identifier of the multi-file property

{% list tabs %}

- JS

    ```js
    document.addEventListener('DOMContentLoaded', function() {
        let propertyIDSelect = 106;
        let propertySelectValueID = 85;

        let propertyIDMultiSelect = 105;
        let propertyMultiSelectValueID = [79, 80, 82];

        let propertyIDFile = 107;
        let propertyFilePathToPicture = 'pictures/1.jpg';

        let propertyIDMultiFile = 108;
        let propertyMultiFilePathToPicture = [
            'pictures/2.jpg',
            'pictures/3.jpg',
            'pictures/4.jpg'
        ];

        let standardPreviewPicturePath = 'pictures/5.jpg';
        let standardDetailPicturePath = 'pictures/6.jpg';

        let arFields = {
            'NAME': 'Example product 2',
            'CURRENCY_ID': 'USD',
            'PRICE': 4900,
            'SORT': 500
        };

        if (propertyIDSelect > 0 && propertySelectValueID > 0) {
            arFields['PROPERTY_' + propertyIDSelect] = propertySelectValueID;
        }

        if (propertyIDMultiSelect > 0 && Array.isArray(propertyMultiSelectValueID) && propertyMultiSelectValueID.length > 0) {
            arFields['PROPERTY_' + propertyIDMultiSelect] = propertyMultiSelectValueID;
        }

        function fileToBase64(filePath) {
            return new Promise((resolve, reject) => {
                fetch(filePath)
                    .then(response => response.blob())
                    .then(blob => {
                        let reader = new FileReader();
                        reader.onloadend = () => resolve(reader.result.split(',')[1]);
                        reader.onerror = reject;
                        reader.readAsDataURL(blob);
                    });
            });
        }

        async function prepareFiles() {
            if (propertyIDFile > 0 && propertyFilePathToPicture) {
                let fileName = propertyFilePathToPicture.split('/').pop();
                arFields['PROPERTY_' + propertyIDFile] = {
                    "fileData": [
                        fileName,
                        await fileToBase64(propertyFilePathToPicture)
                    ]
                };
            }

            if (propertyIDMultiFile > 0 && Array.isArray(propertyMultiFilePathToPicture) && propertyMultiFilePathToPicture.length > 0) {
                arFields['PROPERTY_' + propertyIDMultiFile] = [];
                for (let path of propertyMultiFilePathToPicture) {
                    let fileName = path.split('/').pop();
                    arFields['PROPERTY_' + propertyIDMultiFile].push({
                        "fileData": [
                            fileName,
                            await fileToBase64(path)
                        ]
                    });
                }
            }

            if (standardPreviewPicturePath) {
                let fileName = standardPreviewPicturePath.split('/').pop();
                arFields['PREVIEW_PICTURE'] = {
                    "fileData": [
                        fileName,
                        await fileToBase64(standardPreviewPicturePath)
                    ]
                };
            }

            if (standardDetailPicturePath) {
                let fileName = standardDetailPicturePath.split('/').pop();
                arFields['DETAIL_PICTURE'] = {
                    "fileData": [
                        fileName,
                        await fileToBase64(standardDetailPicturePath)
                    ]
                };
            }

            BX24.callMethod(
                'crm.product.add',
                {
                    'fields': arFields
                },
                function(result) {
                    if (result.error()) {
                        console.error(result.error());
                    } else {
                        console.log('Product added successfully');
                    }
                }
            );
        }

        prepareFiles();
    });
    ```

- PHP

    {% note info %}

    To use the examples in PHP, configure the *CRest* class and include the **crest.php** file in the files where this class is used. [Learn more](../../../how-to-use-examples.md)

    {% endnote %}

    ```php
    <?
        $propertyIDSelect = 106;
        $propertySelectValueID = 85;
        
        $propertyIDMultiSelect = 105;
        $propertyMultiSelectValueID = [79, 80, 82];
        
        $propertyIDFile = 107;
        $propertyFilePathToPicture = 'pictures/1.jpg'; //relative or full path on server
        
        $propertyIDMultiFile = 108;
        $propertyMultiFilePathToPicture = [ //relative or full path on server
            'pictures/2.jpg',
            'pictures/3.jpg',
            'pictures/4.jpg',
        ];
        
        $standardPreviewPicturePath = 'pictures/5.jpg'; //relative or full path on server
        $standardDetailPicturePath = 'pictures/6.jpg'; //relative or full path on server
        
        $arFields = [
            'NAME'        => 'Example product 2',
            'CURRENCY_ID' => 'USD',
            'PRICE'     => 4900,
            'SORT'        => 500
        ];
        
        
        if($propertyIDSelect > 0 && $propertySelectValueID > 0)
        {
            $arFields[ 'PROPERTY_' . $propertyIDSelect ] = $propertySelectValueID;
        }
        
        if($propertyIDMultiSelect > 0 && is_array($propertyMultiSelectValueID) && count($propertyMultiSelectValueID) > 0)
        {
            $arFields[ 'PROPERTY_' . $propertyIDMultiSelect ] = $propertyMultiSelectValueID;
        }
        
        if($propertyIDFile > 0 && !empty($propertyFilePathToPicture) && file_exists($propertyFilePathToPicture))
        {
            $fileName = end(explode('/', $propertyFilePathToPicture));
            $arFields[ 'PROPERTY_' . $propertyIDFile ] = [
                "fileData" => [
                    $fileName,
                    base64_encode(file_get_contents($propertyFilePathToPicture))
                ]
            ];
        }
        
        if($propertyIDMultiFile > 0 && is_array($propertyMultiFilePathToPicture) && count($propertyMultiFilePathToPicture) > 0)
        {
            foreach($propertyMultiFilePathToPicture as $path){
                if(file_exists($path))
                {
                    $fileName = end(explode('/', $path));
                    $arFields[ 'PROPERTY_' . $propertyIDMultiFile ][] = [
                        "fileData" => [
                            $fileName,
                            base64_encode(file_get_contents($path))
                        ]
                    ];
                }
            }
        }
        
        if(!empty($standardPreviewPicturePath) && file_exists($standardPreviewPicturePath))
        {
            $fileName = end(explode('/', $standardPreviewPicturePath));
            $arFields[ 'PREVIEW_PICTURE' ] = [
                "fileData" => [
                    $fileName,
                    base64_encode(file_get_contents($standardPreviewPicturePath))
                ]
            ];
        }
        
        if(!empty($standardDetailPicturePath) && file_exists($standardDetailPicturePath))
        {
            $fileName = end(explode('/', $standardDetailPicturePath));
            $arFields[ 'DETAIL_PICTURE' ] = [
                "fileData" => [
                    $fileName,
                    base64_encode(file_get_contents($standardDetailPicturePath))
                ]
            ];
        }
        
        $result = CRest::call(
            'crm.product.add',
            [
                'fields' => $arFields
            
            ]
        );

    ?>
    ```

{% endlist %}