# Working with s3

In order to push data into the AWS S3 we need the following
* AWS Programattic Access
* Access key ID
* Secret access key

## Steps:

Add the following dependency,
```java
    <dependency>
      <groupId>com.amazonaws</groupId>
      <artifactId>aws-java-sdk-s3</artifactId>
      <version>1.11.836</version>
    </dependency>
```

The Java Class,
```java
package com.aws;

import com.amazonaws.auth.AWSStaticCredentialsProvider;
import com.amazonaws.auth.BasicAWSCredentials;
import com.amazonaws.services.s3.AmazonS3;
import com.amazonaws.services.s3.AmazonS3ClientBuilder;
import com.amazonaws.services.s3.model.AmazonS3Exception;
import com.amazonaws.services.s3.transfer.MultipleFileUpload;
import com.amazonaws.services.s3.transfer.TransferManager;
import com.amazonaws.services.s3.transfer.TransferManagerBuilder;
import org.apache.log4j.Logger;

import java.io.File;

public class S3TransferProcess {
    static Logger log = Logger.getLogger(S3TransferProcess.class.getName());

    private static AmazonS3 s3=null;
    private static  TransferManager tx;
    private static BasicAWSCredentials awsCredentials=null;
    private final static String bucketName="test-kingshuk-1";

    public S3TransferProcess() {
        awsCredentials=new BasicAWSCredentials("accessKey","secretKey");
        AWSStaticCredentialsProvider awsCreds=new AWSStaticCredentialsProvider(awsCredentials);

        s3 = AmazonS3ClientBuilder.standard()
                .withCredentials(awsCreds)
                .withRegion("us-west-2")
                .build();
        tx= TransferManagerBuilder.standard()
                .withS3Client(s3)
                .build();
    }

    public boolean uploadFolderIntoS3(String folderPath,String s3FolderName){
        boolean recursive=false;
        try {
             MultipleFileUpload multipleFileUpload=tx.uploadDirectory(bucketName,s3FolderName,new File(folderPath),recursive);
             multipleFileUpload.waitForCompletion();
             return true;
        }catch (InterruptedException e) {
            e.printStackTrace();
        }catch (AmazonS3Exception e2){
            e2.printStackTrace();
        }
        return false;
    }

    public void shutdownNowTransferManager(){
        tx.shutdownNow();
    }

    /*public static void main(String[] args) {
        String bucketName="test-kingshuk-1";
        //String keyName="Jira_Logs.xlsx";
        String keyPrefix="ss";
        boolean recursive=false;
        String filePath=System.getProperty("user.dir")+"\\logs\\backUp";
        try {
          //Create A Bucket
          //s3.createBucket("test-kingshuk-1");

          //Delete A Bucket
          //s3.deleteBucket("test-kingshuk-1");

          //Putting a single file inside the bucket
          //PutObjectRequest putObj=new PutObjectRequest(bucketName,keyName,new File(filePath));
          //  s3.putObject(putObj);

           //Putting a folder inside a bucket
            MultipleFileUpload multipleFileUpload=tx.uploadDirectory(bucketName,keyPrefix,new File(filePath),recursive);
            multipleFileUpload.waitForCompletion();
        }catch (AmazonS3Exception | InterruptedException e1){
            e1.printStackTrace();
        }
        tx.shutdownNow();
    }*/

    public static void main(String[] args) {
        String keyPrefix="ss";
        boolean recursive=false;
        String folderPath=System.getProperty("user.dir")+"\\logs\\backUp";

        S3TransferProcess s3TransferProcess=new S3TransferProcess();
        s3TransferProcess.uploadFolderIntoS3(folderPath,"ss");
        s3TransferProcess.shutdownNowTransferManager();

    }
}

```
