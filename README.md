### Build
```
clean package shade:shade
```

### Run 
```
sam local invoke -e event.json
```

# Error
```
2018-06-10 03:16:09 Invoking com.github.frkr.DroolsHandler::DroolsHandler (java8)
2018-06-10 03:16:09 Starting new HTTP connection (1): 169.254.169.254
2018-06-10 03:16:11 Decompressing /home/davi/Desktop/drools-lambda/target/drools-lambda.jar

Fetching lambci/lambda:java8 Docker container image......
2018-06-10 03:16:13 Mounting /tmp/tmpiQQthP as /var/task:ro inside runtime container
START RequestId: 90a45aa3-9e5b-40e5-89e8-8c22727eb406 Version: $LATEST
java.lang.ClassNotFoundException: com.github.frkr.DroolsHandler
        at java.net.URLClassLoader.findClass(URLClassLoader.java:381)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
        at java.lang.Class.forName0(Native Method)
        at java.lang.Class.forName(Class.java:348)

END RequestId: 90a45aa3-9e5b-40e5-89e8-8c22727eb406
REPORT RequestId: 90a45aa3-9e5b-40e5-89e8-8c22727eb406  Duration: 5.37 ms       Billed Duration: 100 ms Memory Size: 128 MB Max Memory Used: 4 MB   
```
