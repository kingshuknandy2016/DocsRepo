## Stubs
We can stub a data Service Layer here.

First We are creating an Data Service Interface
```java
public interface SomeDataService {
    int[] retrieveAllData();
}
```

Then we are implemeting the Interface
```java
public class SomeBusinessImplementation {

    private SomeDataService someDataService;

    public SomeDataService getSomeDataService() {
        return someDataService;
    }

    public void setSomeDataService(SomeDataService someDataService) {
        this.someDataService = someDataService;
    }

    public int calculateSum(int[] intArr){
        int sum=0;
        for(int value:intArr){
            sum=sum+value;
        }
        return sum;
    }

    public int calculateSumUsingDataService(){
        int[] intArr=someDataService.retrieveAllData();
        int sum=0;
        for(int value:intArr){
            sum=sum+value;
        }
        return sum;
    }
}
```

Then the test Cases by using stub will be like

```java
class SomeDataServiceImpl implements SomeDataService {

    @Override
    public int[] retrieveAllData() {
        return new int[]{1,2,3};
    }
}

//Stubbing the SomeData Service
class SomeDataServiceEmptyImpl implements SomeDataService {

    @Override
    public int[] retrieveAllData() {
        return new int[]{};
    }
}

public class Test_SomeBusinessStubTests {

    /**
     * @description : Basic Unit Testing. Some Value
     */
    @Test
    public void calculateSumUsingDataService_Basics(){
        SomeBusinessImplementation business=new SomeBusinessImplementation();
        business.setSomeDataService(new SomeDataServiceImpl());

        int actualResult=business.calculateSumUsingDataService();
        int expectedResult=6;
        assertEquals(expectedResult,actualResult);
    }

    /**
     * @description : Basic Unit Testing. Empty Array
     */
    @Test
    public void calculateSum_Basics_empty(){
        SomeBusinessImplementation business=new SomeBusinessImplementation();
        business.setSomeDataService(new SomeDataServiceEmptyImpl());
        int actualResult=business.calculateSumUsingDataService();
        int expectedResult=0;
        assertEquals(expectedResult,actualResult);
    }


}
```


## Mock
By Mocks we can programatically create classes ( Implemetation Classes of the Data Service Interfaces)

```java
public class Test_SomeBusinessMockTests {

    /**
     * @description : Basic Unit Testing. Some Value
     */
    @Test
    public void calculateSumUsingDataService_Basics(){
        SomeBusinessImplementation business=new SomeBusinessImplementation();

        SomeDataService dataServiceMock=mock(SomeDataService.class);   //Mocking the Data service Interface
        // dataServiceMock retrieveAllData new int[]{1,2,3}
        when(dataServiceMock.retrieveAllData()).thenReturn(new int[]{1,2,3});

        // business.setSomeDataService(new SomeDataServiceImpl());  //Using the Stub
        business.setSomeDataService(dataServiceMock);  // Using the Mock object

        int actualResult=business.calculateSumUsingDataService();
        int expectedResult=6;
        assertEquals(expectedResult,actualResult);
    }

    /**
     * @description : Basic Unit Testing. Empty Array
     */
    @Test
    public void calculateSum_Basics_empty(){
        SomeBusinessImplementation business=new SomeBusinessImplementation();

        SomeDataService dataServiceMock=mock(SomeDataService.class);   //Mocking the Data service Interface
        // dataServiceMock retrieveAllData new int[]{}
        when(dataServiceMock.retrieveAllData()).thenReturn(new int[]{});

        //business.setSomeDataService(new SomeDataServiceEmptyImpl());
        business.setSomeDataService(dataServiceMock);
        int actualResult=business.calculateSumUsingDataService();
        int expectedResult=0;
        assertEquals(expectedResult,actualResult);
    }


}
```
If we format the code, in a more formatted way
```java
public class Test_SomeBusinessMockTests {

    SomeBusinessImplementation business=new SomeBusinessImplementation();
    SomeDataService dataServiceMock=mock(SomeDataService.class);

    @Before
    public void before(){
        business.setSomeDataService(dataServiceMock);
    }
    
    @Test
    public void calculateSumUsingDataService_Basics(){
        when(dataServiceMock.retrieveAllData()).thenReturn(new int[]{1,2,3});
        int actualResult=business.calculateSumUsingDataService();
        int expectedResult=6;
        assertEquals(expectedResult,actualResult);
    }

    @Test
    public void calculateSum_Basics_empty(){
        when(dataServiceMock.retrieveAllData()).thenReturn(new int[]{});
        int actualResult=business.calculateSumUsingDataService();
        int expectedResult=0;
        assertEquals(expectedResult,actualResult);
    }
    
}
```
A far more simplified approach
```java
import com.dummy.services.SomeDataService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.runners.MockitoJUnitRunner;

import static org.junit.Assert.assertEquals;
import static org.mockito.Mockito.when;

@RunWith(MockitoJUnitRunner.class)
public class Test_SomeBusinessMockTests {

    @InjectMocks
    SomeBusinessImplementation business=new SomeBusinessImplementation();

    @Mock
    SomeDataService dataServiceMock;

    @Test
    public void calculateSumUsingDataService_Basics(){
        when(dataServiceMock.retrieveAllData()).thenReturn(new int[]{1,2,3});
        int actualResult=business.calculateSumUsingDataService();
        int expectedResult=6;
        assertEquals(expectedResult,actualResult);
    }

    @Test
    public void calculateSum_Basics_empty(){
        when(dataServiceMock.retrieveAllData()).thenReturn(new int[]{});
        int actualResult=business.calculateSumUsingDataService();
        int expectedResult=0;
        assertEquals(expectedResult,actualResult);
    }

}
```

Here autoamatically the inject ***business.setSomeDataService(dataServiceMock); *** will happen

### Return Values

```java
import org.junit.Test;
import java.util.List;
import static org.mockito.Mockito.when;
import static org.mockito.Mockito.mock;
import static org.junit.Assert.assertEquals;

public class Test_ListMockTest {

    List mock= mock(List.class);

    @Test
    public void size_basic(){
        when(mock.size()).thenReturn(5);
        assertEquals(5,mock.size());
    }

    @Test
    public void returnDifferentValues(){
        when(mock.size())
                .thenReturn(5)
                .thenReturn(10);
        assertEquals(5,mock.size());
        assertEquals(10,mock.size());
    }

    @Test
    public void returnWithParameters(){
        when(mock.get(0))
                .thenReturn("Test");
        assertEquals(5,mock.get(0));
        assertEquals(null,mock.get(1));  //Any Other Values other than the default set values will give null
    }
}
```
