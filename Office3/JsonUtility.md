 
 ### Basic Read Json From A json file
 
 Dependency required:
 ```java
         <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.8.5</version>
        </dependency>
 ```
 
 Then method,
 ```java
 public JsonElement getJsonRequestBody(String filename)  {
        JsonElement jsonElement = null;
        try {
            jsonElement = new JsonParser().parse(new FileReader(System.getProperty("user.dir") + "/testdata/requestbody/" + filename));
        } catch (FileNotFoundException e) {
            System.out.println("");
        }
        return jsonElement;
    }
```

Usage:
```java
DataUtility dataUtility=new DataUtility();
        String responseBody=dataUtility.getJsonRequestBody("prodyuctListingByOwner.json").toString();
        
```

 
