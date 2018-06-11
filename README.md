### Build
```
mvn clean package shade:shade
```

### Local Run (sam is broken)
```
sam local invoke -e event.json
```

### LIST Functions
```
aws lambda list-functions
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

### Create API Gateway
```
aws apigateway create-rest-api --name 'DroolsFunction'
```

### Get Resources
```
aws apigateway get-resources --rest-api-id gd4v1lrjjd
```

### Create Resource
```
aws apigateway create-resource --rest-api-id gd4v1lrjjd --parent-id q5nlsd6eu5 --path-part 'drools'
```

### Create API Method
```
aws apigateway put-method --rest-api-id gd4v1lrjjd --resource-id hlv8n6 \
--http-method POST --authorization-type "NONE" --no-api-key-required \
--request-parameters "method.request.header.custom-header=false"
```

### Create Integration
```
aws apigateway put-integration --rest-api-id gd4v1lrjjd --resource-id hlv8n6 \
--http-method POST --type AWS --integration-http-method POST \
--uri 'arn:aws:apigateway:sa-east-1:lambda:path/2015-03-31/functions/arn:aws:lambda:sa-east-1:827387554119:function:DroolsFunction/invocations'

--uri 'arn:aws:lambda:sa-east-1:827387554119:function:DroolsFunction'
```

### Delete or Update
```
 delete-integration \
aws apigateway \
 get-integration \
--rest-api-id gd4v1lrjjd \
--resource-id hlv8n6 \
--http-method POST 
```

### Method response
```
aws apigateway \
put-method-response \
--rest-api-id gd4v1lrjjd \
--resource-id hlv8n6 \
--http-method POST \
--status-code 200
```

### Integration (not working using cli)

```

aws apigateway put-integration-response --rest-api-id gd4v1lrjjd --resource-id hlv8n6 \
 --http-method POST --status-code 200 --selection-pattern "" --response-templates '{"application/json": "{\"json\": \"template\"}"}'


aws apigateway \
 get-integration-response \
--rest-api-id gd4v1lrjjd \
--resource-id hlv8n6 \
--http-method POST \
--status-code 200

aws apigateway create-deployment --rest-api-id gd4v1lrjjd --region sa-east-1

aws apigateway update-stage --region <region> \
    --rest-api-id <rest-api-id> \ 
    --stage-name <stage-name> \ 
    --patch-operations op='replace',path='/deploymentId',value='<deployment-id>'

```


### POST
```
curl -i -X POST -H "Content-Type:application/json" -d "{  \"nome\" : \"Davi\" }" https://gd4v1lrjjd.execute-api.sa-east-1.amazonaws.com/prod/drools
```
