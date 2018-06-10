### Build
```
mvn clean package shade:shade
```

### Local Run (sam is broken)
```
sam local invoke -e event.json
```

### Create Function
```
aws lambda create-function --function-name DroolsFunction --runtime java8 --role arn:aws:iam::827387554119:role/service-role/microservice --handler com.github.frkr.DroolsHandler --zip-file fileb://`pwd`/target/lambda.jar   
```

### Update Function
```
aws lambda update-function-code --function-name DroolsFunction --zip-file fileb://`pwd`/target/lambda.jar
```

### Invoke
```
aws lambda invoke --function-name DroolsFunction --payload '{"teste":"sim"}' output.log
```
