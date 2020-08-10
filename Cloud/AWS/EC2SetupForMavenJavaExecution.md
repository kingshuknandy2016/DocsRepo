# Setup EC2 Instance for Maven

## Set-Up Java:
*  Get the jdk package <br/>
***wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u141-b15/336fa29ff2bb4ef291e347e091f7f4a7/jdk-8u141-linux-x64.rpm***

* Install the Package
  ***yum install -y jdk-8u141-linux-x64.rpm***<br/>
  ***yum install java-1.8.0***
  
* Select the desired JAVA
***/usr/sbin/alternatives --config java***

* Set the Path<br/>
***export JAVA_HOME=/usr/java/jdk1.8.0_141***<br/>
***export JRE_HOME=/usr/java/jdk1.8.0_141/jre***<br/>
***export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin***<br/>

* Check the Version<br/>
***java -version***<br/>
***javac -version***<br/>


