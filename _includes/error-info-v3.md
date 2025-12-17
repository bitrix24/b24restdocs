### Returned Error Information

#|  
|| **Name**  
`type` | **Description** ||  
|| **error.code**  
`string`| String error code. Use it to identify the type of exception ||  
|| **error.message**  
`string`| Text description of the error ||  
|| **error.validation**  
`array`| Array with error details. Present only in data validation errors `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION` ||  
|| **error.validation[].field**  
`string`| Name of the field where the validation error occurred ||  
|| **error.validation[].message**  
`string`| Description of the error related to the specified field ||  
|#  