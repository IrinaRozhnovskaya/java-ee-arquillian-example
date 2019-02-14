# java-ee-arquillian-example

This is a simple example to show how to deploy and test an Arquillian test case on JBoss EAP 6.4.0.
This project is a starting point for using Arquillian. It has a simple CDI test case that runs against Remote and Managed JBoss AS.

**Prerequisites:**

JBoss EAP 6.4.0 installed and running on localhost port 8080

JBOSS_HOME environment variable set

**Running test**

on a remote JBoss AS with JBoss already running 
```bash
mvnw clean test -Parquillian-jbossas-remote
```

on a managed JBoss AS. Maven will handle downloading and extracting the server for you  
```bash
mvnw clean test -Parquillian-jbossas-managed
```

The container adapter for JBoss AS 7 uses the JMX protocol by default rather than the more typical Servlet protocol.
As a result, the web contexts required by scoped CDI beans do not get activated when the test is run. 
The solution is to force the JBoss AS 7 container adapter to use the Servlet protocol. 

arquillian.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<arquillian xmlns="http://jboss.org/schema/arquillian"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="
        http://jboss.org/schema/arquillian
        http://jboss.org/schema/arquillian/arquillian_1_0.xsd">

    <defaultProtocol type="Servlet 3.0"/>

    <container qualifier="jboss" default="true">

    </container>
</arquillian>
```
deployment method
```java
 @Deployment @OverProtocol("Servlet 3.0")
    public static WebArchive createDeployment() {
        return ShrinkWrap.create(WebArchive.class)...
    }
```
pom.xml
```xml
<dependency>
     <groupId>org.jboss.arquillian.protocol</groupId>
     <artifactId>arquillian-protocol-servlet</artifactId>
     <version>1.1.11.Final</version>
     <scope>test</scope>
</dependency>
```
getting started guide
http://arquillian.org/guides/getting_started/

