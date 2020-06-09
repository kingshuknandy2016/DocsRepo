# Working with Property Files

```java
package com.zinier.core.utils;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.util.HashMap;
import java.util.Properties;

public class PropertyFileUtils {

  private HashMap<String, Object> propsData = new HashMap<>();
  private String filePath;

  public PropertyFileUtils(String filePath) {
    this.filePath = filePath;
  }

  public HashMap<String, Object> readPropertyFile() {
    try {
      InputStream inputStream = new FileInputStream(filePath);
      Properties properties = new Properties();
      properties.load(inputStream);
      for (String key : properties.stringPropertyNames()) {
        propsData.put(key, properties.getProperty(key));
      }
    } catch (FileNotFoundException e) {
      e.printStackTrace();
    } catch (IOException e) {
      e.printStackTrace();
    }
   return propsData;
  }

  public void writeIntoPropertyFile(String key,Object value){
    try {
      OutputStream outputStream = new FileOutputStream(filePath);
      Properties prop = new Properties();
      prop.setProperty(key,value.toString());
      prop.store(outputStream,"");
    } catch (FileNotFoundException e) {
      e.printStackTrace();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }

}

```
