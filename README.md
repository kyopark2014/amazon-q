# Amazon Q

## Permission Setting


#### Trusted entities

Role의 Trust relationship에 "qbusiness.amazonaws.com" 추가합니다.


```java
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "qbusiness.amazonaws.com"
                ]
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

### Permissions policies 

태스트시에는 아래와 같은 permission을 추가합니다. 

```java
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "VisualEditor0",
			"Effect": "Allow",
			"Action": [
				"qbusiness:*"
			],
			"Resource": "*"
		}
	]
}
```

상세한것은 [IAM role for Amazon Q data source connectors](https://docs.aws.amazon.com/amazonq/latest/business-use-dg/iam-roles.html#iam-roles-ds)에 따라 아래와 같이 설정합니다.

```java
{
    "Version": "2012-10-17",
    "Statement": [{
            "Sid": "AllowsAmazonQToIngestDocuments",
            "Effect": "Allow",
            "Action": [
                "qbusiness:BatchPutDocument",
                "qbusiness:BatchDeleteDocument"
            ],
            "Resource": "arn:aws:qbusiness:{{region}}:{{source_account}}:application/{{application_id}}/index/{{index_id}}"
        },
        {
            "Sid": "AllowsAmazonQToIngestPrincipalMapping",
            "Effect": "Allow",
            "Action": [
                "qbusiness:PutGroup",
                "qbusiness:CreateUser",
                "qbusiness:DeleteGroup",
                "qbusiness:UpdateUser",
                "qbusiness:ListGroups"
            ],
            "Resource": [
                "arn:aws:qbusiness:{{region}}:{{account_id}}:application/{{application_id}}",
                "arn:aws:qbusiness:{{region}}:{{account_id}}:application/{{application_id}}/index/{{index_id}}",
                "arn:aws:qbusiness:{{region}}:{{account_id}}:application/{{application_id}}/index/{{index_id}}/data-source/*"
            ]
        }
    ]
}
```

## Sync 설정

metadata prefix와 Include prefixes을 설정합니다. 여기서 Include prefixes는 bucket에 문서 )dp 파일의 경로이며, metadata prefix는 metadata 파일의 경로입니다.

![image](https://github.com/kyopark2014/amazon-q/assets/52392004/9a80475b-2cb8-4eee-a887-e736dc2bd455)


Maximum file size는 기본이 50MB입니다. 이것은 Kendra Service Quota에서 [Storage capacity units](https://us-west-2.console.aws.amazon.com/servicequotas/home/services/kendra/quotas/L-E8A56FA5)에서 증설을 요청할 수 있습니다. 하지만 Kendra의 경우에 문서에서 추출되는 text의 크기가 5MB로 제한됩니다.

여기서 S3 동기화를 위한 metadata 예제는 아래와 같습니다.

```java
{
   "Attributes":{
      "_category":"upload",
      "_source_uri":"https://db4un15eorxji.cloudfront.net/docs/1-aws-korea-eis-full-report-korean-1.pdf",
      "_version":"1705299204",
      "_language_code":"ko"
   },
   "Title":"docs/1-aws-korea-eis-full-report-korean-1.pdf",
   "DocumentId":"upload-docs_1-aws-korea-eis-full-report-korean-1.pdf"
}
```
