# ECR の設定

```zsh
# AWS側にリポジトリの登録を行う
REPOSITORY_NAME=hello-world
AWS_REGION=ap-northeast-1

aws ecr create-repository --repository-name ${REPOSITORY_NAME} --region ${AWS_REGION}
{
    "repository": {
        "repositoryArn": "arn:aws:ecr:ap-northeast-1:xxxx:repository/hello-world",
        "registryId": "xxxx",
        "repositoryName": "hello-world",
        "repositoryUri": "xxxx.dkr.ecr.ap-northeast-1.amazonaws.com/hello-world",
        "createdAt": "2022-02-18T05:15:20+00:00",
        "imageTagMutability": "MUTABLE",
        "imageScanningConfiguration": {
            "scanOnPush": false
        },
        "encryptionConfiguration": {
            "encryptionType": "AES256"
        }
    }
}

# これでも確認できる
$ aws ecr describe-repositories --region ${AWS_REGION}

# ECRとdockerのpush先を登録
% aws ecr get-login-password --region ap-northeast-1 --profile default | docker login --username AWS --password-stdin xxxx.dkr.ecr.ap-northeast-1.amazonaws.com
WARNING! Your password will be stored unencrypted in /home/mashiyun/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded

# 認証情報の確認
$ cat ~/.docker/config.json

$ docker tag hello-world:latest xxxx.dkr.ecr.ap-northeast-1.amazonaws.com/hello-world:latest
# 名前とサイズを確認
$ docker images

$ docker push xxxx.dkr.ecr.ap-northeast-1.amazonaws.com/hello-world:latest

# AWS cliから確認
$ aws ecr list-images --repository-name ${REPOSITORY_NAME} --region ${AWS_REGION}

# AWSコンソールからアップされているかを確認する

```
