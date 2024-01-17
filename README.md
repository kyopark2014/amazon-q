# Amazon Q

## Permission Setting

Role의 Trust relationship에 "qbusiness.amazonaws.com" 추가합니다.

Trusted entities

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

## Sync 설정

metadata prefix와 Include prefixes을 설정합니다. 여기서 Include prefixes는 bucket에 문서 파일의 경로이며, metadata prefix는 metadata 파일의 경로입니다.

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
