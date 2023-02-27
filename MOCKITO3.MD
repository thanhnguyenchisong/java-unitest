# MOCKITO
### Mocking Theory
Mockito to mock the dependencies.

In the case the method for testing is big complex with many dependencies, so when run the unit test it can be fail by dependencies, It's from our target testing - we would like to test for scope of our logics, source code in our method only, not for dependencies. We want the testes to fail only when the method contains bugs.
So the solution, insteal of using real dependencies, we can use dummy dependencies.
We use mock method from **Mockito** to create dummy objects, that behave as real dependencies

1. Have to mock the object
2. Mock object can be use in mock method and result of method in side of object. 
3. **Default values of mock method**
 -  empty - return type = List
 - null - return type = Object
 - 0/false primitives
### 1. Return the custom values
 ``when(service.getMethod()).thenReturn(value) ``
### 2. Multiple return call
``when(service.getMethod()).thenReturn(value).thenReturn(value1) ``
First call to method that will return value, second time call to method will return value1
### 3. Throw exception
``when(service.getMethod()).thenThrow(BussinessException.class)``
### 4. Angument matcher
``when(service.getMethod(any(), anyDouble())).thenThrow(BussinessException.class)``

It will accept for all anguments for method then throw the exception.
In the case with would like to match a any() angument and next one agument with exact value we have us like blow:
``when(service.getMethod(any(), eq(400))).thenThrow(BussinessException.class)``

``anyString()`` => not match null object, so if you would like send null to that agument - use ``any()``

### 5. Verifying Behaiviors
-  Verify the mock method was called with exactly the expected input
``verify(service).method(arg, arg1)``
- How many time the mock method was called or just a time for calling
``verify(service, time(1)).method(arg, arg1);``
- Never call to a mock method
``verify(service, never()).method(arg, arg1);``
- No more call to service
``verifyNoMoreInteractions(service);``

### 6. Spy
Use ``spy`` - we will call the actual methods from actual classes. So in situation spy method also should be return our dummy object we can use
``
doReturn(expectedObject).when(service).method(arg);
``
So when you use ``spy`` for service then the ``mock`` 's services can use it as dependencies and it will call the real method and logic, so you have to do a script to create dummy object for ``spy`` 's method

### 7. **Differnt bw ``mock`` and ``spy``**
- ``mock`` create fully mock or dummy objects and default behavor when not stubbed is do nothing <=> ``spy`` create a parital mock objects and default hehavior when not stubbed is the real method behavior.
- ``stub`` test-response objects containing specific data and use it to provide replies or we can say stub do nothing more than return the value that we need for the test.

- In ``mock`` and ``spy`` we also use the stub by ``When...`` or ``doReturn...``. In this case we call it is **mock stub** or **spy stub**. Not stub only

### 8. Testing void method
Can not use the ``When`` method with void method
- Use ``doThrow...`` and ``doReturn..`` and ``doNothing`` method 

**What different bw when first and do first ???**
### 8. Argument Captor
``ArgumentCaptor<T> argumentCaptor = AngumentCaptor.forClass(T.class);
``
create argumentCaptor;
In the verify method we can sen this captor as argument

``verify(service), time(1)).method(eq(arg), argumentCaptor.capture());
assertEquals(expectedValue, argumentCaptor.getValue());
``
So the argument captor capture the argument value in that possible and keep it in captor so you can get value to do your checking. We use it in the case we don't know what argument will be passed to that method because that method run inside of another mock method and we just control the input of mock method only.

### 9. Anotations
- @Mock: create a mock object
- @Spy: create a parital mock  object
- @InjectMock: create instance which will use all mock and spy which were create by @Mock and @Spy
- @Captor
### 10. Mockito BDD
Mockito provides BDDMockito to you can write readable source code with pattern **given-when-then**

``
import static org.mockito.BDDMockito.*;
...
given(service.method()).willReturn(value); //given
int value = service.getMethod(); //when
then(service).should(...).method(arg, arg1); //then
``
### 11. Strict Stubbing
If you define some behavior of the class but you never use it later on the case, Mockito will detect such unnecessary behavior definition and throw exception.
=> It is strict stubbing

If you would like to pass that strict stubbing (No exception). Uses:

``lenient().when....``

### 12. Mocking static method
``MockStatic`` since version 3.4.0
- In the past we use PowerMock to mock the static method but don't need any more since mockito 3.4.0
    Beside that if you can not update your Mockito lib >= 3.4.0 you have to use one more lib that is Powermock
Use lib mockito-inline

``
MockStatic<CurrencyConverter> mockConverter = mockStatic(CurrencyConverter.class);
mockConverter.when(() -> CurrencyConverter.staticMethod(arg)).thenReturn(expected);
double actual = CurrencyConverter.staticMethod(arg);
assertEqual(expected, actual);
``

### 12. Use Mockito Answers
If you would like return the value base on the inputs in the static mock stub

``
mockConverter.when(() -> service.staticMethod(anyDouble())).thenAnswer(inv -> (double)inv.getArgument(0) * 0.8);
``
### 13. Mocking final and private method
1. Final method
   Can mock by mockito-inline version
   Can't mock by mockito-core version
2. Private method
   Can't mock private method.
   From the standpoint of testing... private methods don't exist so don't need to try mock them
   But in the case you must test on private method we can use method invocation in Java class. It's the way to access and execute your private method.

   ## SUMMARY
1. Understand mocking theory
2. Create mocks and spies
3. Mock method
4. Appy mat chers and argument captors
5. USe the verify() method
6. Work with anotations, BDD style methos and strict stubbling
7. Mock static and final method.