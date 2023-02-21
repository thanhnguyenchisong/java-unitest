# UNIT TEST
#### What are unit test
- Tests at the lowest level
- Test single units of software
- This typically means methods
#### Unit tests necessary ?
- Keep the quanlity of software
- Increased confidence during development
- Cleaner, more reusable code
- Lower cost of mistake.

#### Best practices
We have some name pattern for unitest test method, it should clear overview the target of unit test.

convenstion.

``
public void should_ReturnTrue_When_DietRecommended {
    //given
        ...
    //when
        ...
    //then
        ...
}
``

#### Test exceptions
Can use assertThrows(Exception.class, ...)
``
 public void should_ReturnTrue_When_DietRecommended {
    //given
        double weight = 50.0;
        double height = 0.0;
    //when
        Executable e = () -> BMICalculator.isDietRecommended(weight, geight)
    //then
       assertThrows(Exception.class, e);
}
``

#### Test with multiple assertions
Can use many assertions in a method - if it pass all of assertions => It works fine
if we write is as sequently assertion1; assertion2; then if assention 1 be false then application will never go to assertion2.
``
   assertion1;
    assertion2;
``
=> We don't know anything about assertion2. 
So we should use assertAll
``
 assertAll(
  () => assertion1;
  () => assertion2;
 );
``
This way will run all assertions 1 and 2, then we can fix it in a time and have better overview of error of this method

#### Test null value
Can use assertNull
#### Test array equality
AssertArrayEquals(expected, current); this check the values inside of array only, don't check the address.
#### @BeforeEach and @AfterEach
It will run the method which have this anotation before and after test method.
@BeforeEach method can be used as  setup something before go to the method.
@AfterEach method can be used to do something ater finish method.

#### @BeforeAll and @AfterAll
Same as name meanin, just run at before run unitest and all unit test have finished

# ADVANCED CONCEPTS

#### Parameterized Tests - ValueSource
It just do for a value inside of source code only
``
@ParameterizedTest
@ValueSource(doubles = {70.0, 60.0})
void should_ReturnTrue_When_DietRecommended(Double coderWeight) {
    //given
    double weight = coderWeight;
    double height = 1.72;
    //when
    //then
}
``
This unitest will run with parameter frrom @ValueSource. Each of value in @ValueSource will run a time - So that mean this unitest will run 2 times with 2 different input is 70.0 and 60.0

#### Parameterized Tests - CsvSource

``
@ParameterizedTest
@CsvSource(doubles = {"70.0, 1.0", "60.0, 2.0"})
void should_ReturnTrue_When_DietRecommended(Double coderWeight, Double coderHeight) {
    //given
    double weight = coderWeight;
    double height = coderHeight;
    //when
    //then
}
``
This unitest will run with parameter. each of group value in CsvSource will run a time - so we have 2 group "70.0, 1.0", "60.0, 2.0"

To friendly wat we can add the name to display in console to @ParameterizedTest(name ="weight={0}, height={1}")

#### Parameterized Testes - CsvFileSource
The file should have the format
weight, height
1,2
3,4
``
@ParameterizedTest(name ="weight={0}, height={1}")
@CsvFileSource(resource="/test-data.csv")
void should_ReturnTrue_When_DietRecommended(Double coderWeight, Double coderHeight) {
    //given
    double weight = coderWeight;
    double height = coderHeight;
    //when
    //then
}
``

#### Repeated tests
Use it in the case the data given that is random so each of time to testing will run with a diffent input
``
@RepeatedTest(10)
void should_ReturnTrue_When_DietRecommended(Double coderWeight, Double coderHeight) {
    //given
    double weight = coderWeight;
    double height = coderHeight;
    //when
    //then
}
``

#### Test performance
assertTimeout(Duration.ofMillis(1), executableFunction);

#### Assumptions
To decide skip or run the unit test
``
private this.evironment =  "dev";
assumeTrue(this.evironment.equals("prod")); //=> value false -> 
not false in this unitest just skip this unit test. If this.evironment = "prod" the unitest will run
``

#### @Nested
We can group all unitest that have same idea to a nested class and that class use @Nested anotation.
This is a good orgnazation

#### Othe anotation
@Displayname("name is thanh") : a customize name for a unit test or for a nested class which have @Nested

@Disable(): skip the unitest orr Nested unitest class

@DisableOnOs(OS.Windows): skip by OS

#### JUnit 4 and Junite 5

Junit 4 tests can run on JUnit5 but no back-competible.

***Anotaion***
JUnit 4 -> JUnit 5
@Before -> @BeforeEach
@After -> @AfterEach
@BeforeClass -> @BeforeAll
@AfterClass -> @AfterAll
@Ignore -> Disable

***Method name***
JUnit 4 must have public in method name
Junit 5 no need for public in method name

***Test Expection***
JUnit 4
@Test(expected = Exception.class)

JUnit 5 
assertThrows(Exception.class, () -> {})

***Test performance***
J4: @Test(timeout =1)
J5: assertTimeout()

***@Nested and @RepeatedTest is not available in JUnit4***

# Test-Driven Development (TDD)
Creating software by starting with unit tests that false and only then writing the actual code

**Why should use it**
- Increased code quality
- Unit tests are alaways ready as soon as you finish your implementation
- Modern and agile approach