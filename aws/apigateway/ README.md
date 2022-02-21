## test ログ

```text
リクエスト: /
ステータス: 502
レーテンシー: 407 ms
レスポンス本文
{
  "message": "Internal server error"
}
レスポンスヘッダー
{"x-amzn-ErrorType":"InternalServerErrorException"}
ログ
Execution log for request e0b897c0-dce5-4c64-b426-c897c1d225b5
Fri Feb 18 09:07:20 UTC 2022 : Starting execution for request: e0b897c0-dce5-4c64-b426-c897c1d225b5
Fri Feb 18 09:07:20 UTC 2022 : HTTP Method: GET, Resource Path: /
Fri Feb 18 09:07:20 UTC 2022 : Method request path: {}
Fri Feb 18 09:07:20 UTC 2022 : Method request query string: {}
Fri Feb 18 09:07:20 UTC 2022 : Method request headers: {}
Fri Feb 18 09:07:20 UTC 2022 : Method request body before transformations:
Fri Feb 18 09:07:20 UTC 2022 : Endpoint request URI: https://lambda.ap-northeast-1.amazonaws.com/2015-03-31/functions/arn:aws:lambda:ap-northeast-1:262334296846:function:hello-world/invocations
Fri Feb 18 09:07:20 UTC 2022 : Endpoint request headers: {X-Amz-Date=20220218T090720Z, x-amzn-apigateway-api-id=v0z2jhb4fd, Accept=application/json, User-Agent=AmazonAPIGateway_v0z2jhb4fd, Host=lambda.ap-northeast-1.amazonaws.com, X-Amz-Content-Sha256=358491289262665b0a303fbbd3e9a12d939c762e0eab899c981b07959df468cd, X-Amzn-Trace-Id=Root=1-620f61c8-65a4123de94ad2e999aa425f, x-amzn-lambda-integration-tag=e0b897c0-dce5-4c64-b426-c897c1d225b5, Authorization=**************************************************************************************************************************************************************************************************************************************************************************************************************************************************7af421, X-Amz-Source-Arn=arn:aws:execute-api:ap-northeast-1:262334296846:v0z2jhb4fd/test-invoke-stage/GET/, X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD//////////wEaDmFwLW5vcnRoZWFzdC0xIkcwRQIgO5PuQadY2fZzrCycl7SsLo+1p0ihwys0/+9oiVGSg50CIQCk9OTMp+nolS8GNDkBoLlQ64 [TRUNCATED]
Fri Feb 18 09:07:20 UTC 2022 : Endpoint request body after transformations: {"resource":"/","path":"/","httpMethod":"GET","headers":null,"multiValueHeaders":null,"queryStringParameters":null,"multiValueQueryStringParameters":null,"pathParameters":null,"stageVariables":null,"requestContext":{"resourceId":"5zlqdhz4ef","resourcePath":"/","httpMethod":"GET","extendedRequestId":"Nuw3WEcbtjMF3RA=","requestTime":"18/Feb/2022:09:07:20 +0000","path":"/","accountId":"262334296846","protocol":"HTTP/1.1","stage":"test-invoke-stage","domainPrefix":"testPrefix","requestTimeEpoch":1645175240435,"requestId":"e0b897c0-dce5-4c64-b426-c897c1d225b5","identity":{"cognitoIdentityPoolId":null,"cognitoIdentityId":null,"apiKey":"test-invoke-api-key","principalOrgId":null,"cognitoAuthenticationType":null,"userArn":"arn:aws:iam::262334296846:user/iam-user","apiKeyId":"test-invoke-api-key-id","userAgent":"aws-internal/3 aws-sdk-java/1.12.154 Linux/5.4.156-94.273.amzn2int.x86_64 OpenJDK_64-Bit_Server_VM/25.322-b06 java/1.8.0_322 vendor/Oracle_Corporation cfg/retry-mod [TRUNCATED]
Fri Feb 18 09:07:20 UTC 2022 : Sending request to https://lambda.ap-northeast-1.amazonaws.com/2015-03-31/functions/arn:aws:lambda:ap-northeast-1:262334296846:function:hello-world/invocations
Fri Feb 18 09:07:20 UTC 2022 : Received response. Status: 200, Integration latency: 392 ms
Fri Feb 18 09:07:20 UTC 2022 : Endpoint response headers: {Date=Fri, 18 Feb 2022 09:07:20 GMT, Content-Type=application/json, Content-Length=30, Connection=keep-alive, x-amzn-RequestId=cb320022-ee50-4349-b162-0db57b507192, x-amzn-Remapped-Content-Length=0, X-Amz-Executed-Version=$LATEST, X-Amzn-Trace-Id=root=1-620f61c8-65a4123de94ad2e999aa425f;sampled=0}
Fri Feb 18 09:07:20 UTC 2022 : Endpoint response body before transformations: {"Answer:":" is 0 years old!"}
Fri Feb 18 09:07:20 UTC 2022 : Execution failed due to configuration error: Malformed Lambda proxy response
Fri Feb 18 09:07:20 UTC 2022 : Method completed with status: 502
```
