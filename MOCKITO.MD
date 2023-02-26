# MOCKITO
Mockito to mock the dependencies.
1. Have to mock the object
2. Mock object can be use in mock method and result of method in side of object.
Default values of mock method
 1. empty - return type = List
 2. null - return type = Object
 3. 0/false primitives
### Return the custom values**
 ``when(service.getMethod()).thenReturn(value) ``
### Multiple return call
``when(service.getMethod()).thenReturn(value).thenReturn(value1) ``
First call to method that will return value, second time call to method will return value1
### Throw exception
``when(service.getMethod()).thenThrow(BussinessException.class)``
### Angument matcher
``when(service.getMethod(any(), anyDouble())).thenThrow(BussinessException.class)``
It will accept for all anguments for method then throw the exception.
In the case with would like to match a any() angument and next one agument with exact value we have us like blow:
``when(service.getMethod(any(), eq(400))).thenThrow(BussinessException.class)``

``anyString()`` => not match null object, so if you would like send null to that agument - use any()

### Verifying Behaiviors

