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