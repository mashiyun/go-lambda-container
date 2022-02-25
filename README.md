# go-lambda-container

golang と AWS lambda コンテナのサンプル

# 概要

- app1 は go の基本サンプル

# ディレクトリ定義

https://github.com/golang-standards/project-layout/blob/master/README_ja.md

# Reday

go modules について\
https://qiita.com/propella/items/e49bccc88f3cc2407745

```zsh
go mod init github.com/mashiyun/go-lambda-container

# moduleを追加した場合、整理する場合
go mod tidy

# go.mod を更新し、最新のマイナーバージョン、パッチバージョンをキャッシュにダウンロードする
go get -u

```

Docker イメージの作成\
ビルド時にエラーとなることがある。VPN を通すことによって解決できた。（ドメイン制限されている？）

```
#=>(553) % docker build -f ./build/local2/Dockerfile -t hello-world-lambda-local .
Sending build context to Docker daemon  2.309MB
Step 1/14 : FROM public.ecr.aws/lambda/provided:al2 as build
 ---> 02a0268b5d1a
Step 2/14 : RUN yum install -y golang
 ---> Running in a10bc993f725
Loaded plugins: ovl
Could not retrieve mirrorlist http://amazonlinux.default.amazonaws.com/2/core/latest/x86_64/mirror.list error was
12: Timeout on http://amazonlinux.default.amazonaws.com/2/core/latest/x86_64/mirror.list: (28, 'Resolving timed out after 5003 milliseconds')


 One of the configured repositories failed (Unknown),
 and yum doesn't have enough cached data to continue. At this point the only
 safe thing yum can do is fail. There are a few ways to work "fix" this:

     1. Contact the upstream for the repository and get them to fix the problem.

     2. Reconfigure the baseurl/etc. for the repository, to point to a working
        upstream. This is most often useful if you are using a newer
        distribution release than is supported by the repository (and the
        packages for the previous distribution release still work).

     3. Run the command with the repository temporarily disabled
            yum --disablerepo=<repoid> ...

     4. Disable the repository permanently, so yum won't use it by default. Yum
        will then just ignore the repository until you permanently enable it
        again or use --enablerepo for temporary usage:

            yum-config-manager --disable <repoid>
        or
            subscription-manager repos --disable=<repoid>

     5. Configure the failing repository to be skipped, if it is unavailable.
        Note that yum will try to contact the repo. when it runs most commands,
        so will have to try and fail each time (and thus. yum will be be much
        slower). If it is a very temporary problem though, this is often a nice
        compromise:

            yum-config-manager --save --setopt=<repoid>.skip_if_unavailable=true

Cannot find a valid baseurl for repo: amzn2-core/2/x86_64
The command '/bin/sh -c yum install -y golang' returned a non-zero code: 1
```

```zsh
# ビルド(local1)
$ docker build -f ./build/local1/Dockerfile -t hello-world-lambda-local .

# ローカル起動確認
% docker run -p 9000:8080 hello-world-lambda-local:latest /main              <mashiyun> 7:14:36
2022/02/16 07:18:33 expected AWS Lambda environment variables [_LAMBDA_SERVER_PORT AWS_LAMBDA_RUNTIME_API] are not defined

# エラーが出る。Lambdaのパラメータが設定されていないのが原因のようだ。

# ビルド(local2)。RIEが入っているバージョン
$ docker build -f ./build/local2/Dockerfile -t hello-world-lambda-local .
$ docker run -p 9000:8080 hello-world-lambda-local:latest /main
16 Feb 2022 07:37:11,150 [INFO] (rapid) exec '/main' (cwd=/var/task, handler=)

# 起動できる

# 別のターミナルを開いて動作を確認
$ % curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{"What is your name?": "mashiyun", "How old are you?": 150}'  | jq .
{
  "Answer:": "mashiyun is 150 years old!"
}

# 結果を取得できる

# dockerイメージの軽量化を試す

# ビルド(local3)
$ docker build -f ./build/local3/Dockerfile -t hello-world-lambda-local .

# サイズの確認
$ docker images
hello-world-lambda-local             latest    a82e971edd53   10 seconds ago   25MB
<none>                               <none>    c8615b42673f   14 seconds ago   598MB

# local2では300M程度だったが1/10になっている
$ docker run -p 9000:8080 hello-world-lambda-local:latest /main
17 Feb 2022 16:03:20,442 [INFO] (rapid) exec '/main' (cwd=/, handler=)

# 同じように起動できた
# 動作に問題ないようであれば、リリース用のイメージを作成する

# ビルド(prod1)
$ docker build -f ./build/prod1/Dockerfile -t hello-world .

$ docker images
hello-world                          latest    3c2c94462543   5 seconds ago   13.9MB
# さらに容量が小さい！

# ビルド(local4)
$ docker build -f ./build/local4/Dockerfile -t gin-lambda-local .
docker run -p 9000:8080 gin-lambda-local:latest /main

# ローカルでの動作確認。api-gatewayの動作もRIEで再現できる。
$ curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{"resource":"/{proxy+}","path":"/test","httpMethod":"GET","headers":null,"multiValueHeaders":null,"queryStringParameters":null,"multiValueQueryStringParameters":null,"stageVariables":null,"requestContext":{"resourcePath":"/{proxy+}","httpMethod":"GET","path":"/{proxy+}"}}'  | jq .
[GIN-debug] GET    /test                     --> main.init.0.func1 (3 handlers)
[GIN-debug] GET    /                         --> main.init.0.func2 (3 handlers)
[GIN-debug] GET    /ping                     --> main.init.0.func3 (3 handlers)

# ビルド(prod2)
$ docker build -f ./build/prod2/Dockerfile -t gin-lambda .
```

# 参考

https://github.com/awslabs/aws-lambda-go-api-proxy\
https://github.com/aws/aws-lambda-runtime-interface-emulator\
https://github.com/aws-samples/aws-cdk-examples\
https://github.com/aws-samples/aws-cdk-lambda-container\
