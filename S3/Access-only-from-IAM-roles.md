## Restrict S3 bucket access only to specific IAM roles? 
Its easy to restrict S3 bucket access only for certain IAM users but for **roles**, its a bit tricky. Here is the steps for getting this done.

#### Generate role id
The following get-role command gets role id about the roles:


```
aws iam get-role --role-name ROLE_NAME
```
Output:
```
{
  "Role": {
    "AssumeRolePolicyDocument": "<URL-encoded-JSON>",
    "RoleId": "AIDIODR4TAW7CSEXAMPLE",
    "CreateDate": "2019-01-25T05:01:58Z",
    "RoleName": "ROLE_NAME",
    "Path": "/",
    "Arn": "arn:aws:iam::123456789012:role/ROLE_NAME"
  }
}
```

#### Associate bucket policy
Select bucket, navigate to permission and bucket policy
```
{
  "Version": "2012-10-17",
  "Id": "Policy1548375578554",
  "Statement": [
    {
      "Sid": "Stmt1548375576478",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::BUCKETNAME",
        "arn:aws:s3:::BUCKETNAME/*"
      ],
      "Condition": {
        "StringNotLike": {
          "aws:userid": [
            "AIDIODR4TAW7CSEXAMPLE1:*",
            "AIDIODR4TAW7CSEXAMPLE2:*"
          ]
        }
      }
    }
  ]
}
```

#### AWS cli
```
aws s3api put-bucket-policy --bucket MyBucket --policy file://policy.json
```
