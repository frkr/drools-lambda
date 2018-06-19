# Exemplo de Lambda

1) Para a construção de um Lambda" é necessário o descritor de pacote "template.yaml".

Lembrando que a classe que implementa "RequestStreamHandler" será "Singleton" até a JVM sair da instancia (kill).

```yaml
AWSTemplateFormatVersion: 2010-09-09
Transform: AWS::Serverless-2016-10-31

Resources:
  DroolsFunction:
    Type: 'AWS::Serverless::Function'
    Properties:
      Handler: 'com.github.frkr.DroolsHandler'
      CodeUri: ./target/lambda.jar
      MemorySize: 256
      Timeout: 60
      Runtime: java8
```

O pacote abaixo foi para suprir a necessidade de log.

```xml
<dependency>
    <groupId>io.symphonia</groupId>
    <artifactId>lambda-logging</artifactId>
    <version>1.0.0</version>
</dependency>
```

2) Usando o "Maven Shade", fará um pacote único de jar.

Terminando o exemplo de Lambda, continuei com o exemplo de Drools.

# Drools

O arquivo "kmodule.xml" foi necessário para o Drools funcionar. Não testei se as regras podem ficar em outro diretório que não seja o "rules/". O pacote das regras esta como "com.sample" para exemplificar o DRL com o uso de "import".

Neste exemplo, o Drools executa as regras no classpath simples assim:

```java
KieServices ks = KieServices.Factory.get();
KieContainer kContainer = ks.getKieClasspathContainer();
KieSession kSession = kContainer.newKieSession("ksession-rules");

kSession.fireAllRules();
```

TODO: Fazer o Lambda receber um JSON com o DRL, executar e devolver outro JSON.

---
# Comandos úteis:

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
