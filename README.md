# AWS + LOCALSTACK

## IAM ALIASES

 1. ### CREATE ALIASES

```
aws iam create-account-alias --account-alias sakib_aws --endpoint-url http://localhost:4566 --no-sign-request
```
 2. ### GET ALIASES

```
aws iam list-account-aliases --endpoint-url http://localhost:4566 --no-sign-request
```

 3. ### DELETE ALIASES

```
aws iam delete-account-alias --account-alias sakib_aws --endpoint-url http://localhost:4566 --no-sign-request
```


## IAM USERS

 ###  1.LIST USERS

```
 aws iam list-users --endpoint-url http://localhost:4566 --no-sign-request
```

Result 

    {
        "Users": [
            {
                "Path": "/",
                "UserName": "sakib7575",
                "UserId": "83houbbbubtvzm6zoec7",
                "Arn": "arn:aws:iam::000000000000:user/sakib7575",
                "CreateDate": "2023-11-26T15:54:56.239000+00:00"
            }
        ]
    }


###  2.1 CREATE USER

```
aws iam create-user --user-name sakib_developer --path /developer/ --endpoint-url http://localhost:4566 --no-sign-request
```

###  2.2 CREATE USER WITH PATH

```
aws iam create-user --user-name sakib_aws --endpoint-url http://localhost:4566 --no-sign-request
```

###  2.3 CREATE USER WITH TAG

```
aws iam create-user   --user-name sakib_dev_aws --tags '{\"Key\": \"Department\", \"Value\": \"Development\"}' '{\"Key\": \"Location\", \"Value\": \"Dhaka\"}' --endpoint-url http://localhost:4566 --no-sign-request
```
###  2.4 CREATE USER WITH PERMISSION BOUNDARY

```
aws iam create-user --user-name sakib     --permissions-boundary arn:aws:iam::aws:policy/AmazonS3FullAccess --endpoint-url http://localhost:4566 --no-sign-request
```



## ROLE

### CREATE ROLE

```
aws iam create-role --role-name IAMReadOnlyRole --assume-role-policy-document file://iamrole.json --endpoint-url http://localhost:4566 --no-sign-request
```

AssumeRolePolicyDocument JSON FILE


> {
>     "Version": "2012-10-17",
>     "Statement": [
>       {
>         "Effect": "Allow",
>         "Action": [
>           "iam:Get*",
>           "iam:List*"
>         ],
>         "Resource": "*"
>       }
>     ]   }



  ```
  aws iam attach-role-policy --role-name IAMReadOnlyRole --policy-arn arn:aws:iam::aws:policy/ReadOnlyAccess --endpoint-url http://localhost:4566 --no-sign-request
```


### GET ALL ROLES
```
aws iam list-roles --endpoint-url http://localhost:4566 --no-sign-request
```

    {
    "Roles": [
        {
            "Path": "/",
            "RoleName": "IAMDemoRole",
            "RoleId": "AROAQAAAAAAACKTUMBGOA",
            "Arn": "arn:aws:iam::000000000000:role/IAMDemoRole",
            "CreateDate": "2023-11-26T17:23:32.757984+00:00",
            "AssumeRolePolicyDocument": {
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Effect": "Allow",
                        "Action": [
                            "iam:Get*",
                            "iam:List*"
                        ],
                        "Resource": "*"
                    }
                ]
            },
            "Description": "iam demo role",
            "MaxSessionDuration": 3600
        },
        {
            "Path": "/",
            "RoleName": "IAMReadOnlyRole",
            "RoleId": "AROAQAAAAAAAFPKNUGC7S",
            "Arn": "arn:aws:iam::000000000000:role/IAMReadOnlyRole",
            "CreateDate": "2023-11-26T16:28:08.029719+00:00",
            "AssumeRolePolicyDocument": {
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Effect": "Allow",
                        "Action": [
                            "iam:Get*",
                            "iam:List*"
                        ],
                        "Resource": "*"
                    }
                ]
            },
            "MaxSessionDuration": 3600
        }
    ]
    }


## GROUP

### CREATE GROUP

```
aws iam create-group --group-name Developers --endpoint-url http://localhost:4566 --no-sign-request
```
### GET ALL GROUPS

```
aws iam list-groups --endpoint-url http://localhost:4566 --no-sign-request
```
### ADD USER TO GROUP

```
aws iam add-user-to-group --user-name sakib_developer --group-name Developers --endpoint-url http://localhost:4566 --no-sign-request
```
### GET GROUP MEMBERS

```
aws iam get-group --group-name Developers --endpoint-url http://localhost:4566 --no-sign-request
```

### ATTACH POLICY TO A GROUP

```
aws iam attach-group-policy --group-name Admins --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess --endpoint-url http://localhost:4566 --no-sign-request
```

### GET  ATTACHED POLICIES OF A GROUP

```
aws iam list-attached-group-policies --group-name Admins --endpoint-url http://localhost:4566 --no-sign-request
```


### CREATE A USER, THEN ASSIGN THAT USER IN A GROUP AND THEN ACCESS RESOURCES WITH THAT USER

```
aws iam create-user --user-name test_user --endpoint-url http://localhost:4566 --no-sign-request
```


```
aws iam create-group --group-name Developers --endpoint-url http://localhost:4566 --no-sign-request
```


```
aws iam add-user-to-group --user-name test_user --group-name Developers --endpoint-url http://localhost:4566 --no-sign-request

```


```
aws iam create-access-key --user-name test_user --endpoint-url http://localhost:4566 --no-sign-request
```

    {
    "AccessKey": {
        "UserName": "test_user",
        "AccessKeyId": "LKIAQAAAAAAAEBZNGPKY",
        "Status": "Active",
        "SecretAccessKey": "NHMJxRq9dY/d7ls0+l/uUGAjXPgazzPLOjDk7fxH",
        "CreateDate": "2023-11-30T17:11:04Z"
    }
    }


Create S3 Bucket
```
aws s3api create-bucket --bucket mys3bucket --region ap-east-1 --endpoint-url http://localhost:4566 --no-sign-request
```

Create Policy
```
aws iam create-policy --policy-name S3AccessPolicy --policy-document '{"Version":"2012-10-17","Statement":[{"Effect":"Allow","Action":"s3:*","Resource":"arn:aws:s3:::mys3bucket/*"}]}' --endpoint-url http://localhost:4566 --no-sign-request
```


Attach Policy to A Group
```
aws iam attach-group-policy --group-name Developers --policy-arn arn:aws:iam::000000000000:policy/S3AccessPolicy --endpoint-url http://localhost:4566 --no-sign-request
```


Get Attched Policy of A Group
```
aws iam list-attached-group-policies --group-name Developers --endpoint-url http://localhost:4566 --no-sign-request
```


Configure Credentials
```
aws configure set aws_access_key_id LKIAQAAAAAAAEBZNGPKY

aws configure set aws_secret_access_key NHMJxRq9dY/d7ls0+l/uUGAjXPgazzPLOjDk7fx
```
```
aws s3 ls s3://mys3bucket
```
