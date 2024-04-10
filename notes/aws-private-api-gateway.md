# AWS Private API Gateway

## Use Case:
Internal systems calling against API Gateways through a Privatelink interface, prevents external access to API Gateway.

## Notes
Private Gateways need to be associated with a endpoint, have a resource policy granting access and accessed using a specially formatted URL.

The URL format is:
```
https://{rest-api-id}-{vpce-id}.execute-api.{region}.amazonaws.com/{stage}
```

>[!NOTE]
>AWS recommend enabling dns name resolution, this actually prevents external name resolution for API Gateways within the VPC so it's best not to do that, see this [blog article](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-private-api-test-invoke-url.html#apigateway-private-api-route53-alias)

#### To create a VPC Endpoint for API Gateway:
```
aws ec2 create-vpc-endpoint --vpc-id <vpc-id> --service-name "com.amazonaws.<region>.execute-api" --vpc-endpoint-type Interface
```



#### Create a Private API Gateway:
```
aws apigateway create-rest-api \
        --name '<API Name>' \
        --description '<Description Text>' \
        --region <region> \
        --endpoint-configuration '{ "types": ["PRIVATE"] }'
```