# sam-sample

This is a sample template for sam-sample - Below is a brief explanation of what we have generated for you:

```bash
.
├── README.md                   <-- This instructions file
├── hello-world                 <-- Source code for a lambda function
│   ├── main.go                 <-- Lambda function code
│   └── main_test.go            <-- Unit tests
│   └── Dockerfile              <-- Dockerfile
└── template.yaml
```

## Requirements

- AWS CLI already configured with Administrator permission
- [Docker installed](https://www.docker.com/community-edition)
- SAM CLI - [Install the SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)

You may need the following for local testing.

- [Golang](https://golang.org)

## Setup process

### Installing dependencies & building the target

In this example we use the built-in `sam build` to build a docker image from a Dockerfile and then copy the source of your application inside the Docker image.  
Read more about [SAM Build here](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-build.html)

### Local development

**Invoking function locally through local API Gateway**

```bash
sam local start-api
```

If the previous command ran successfully you should now be able to hit the following local endpoint to invoke your function `http://localhost:3000/hello`

**SAM CLI** is used to emulate both Lambda and API Gateway locally and uses our `template.yaml` to understand how to bootstrap this environment (runtime, where the source code is, etc.) - The following excerpt is what the CLI will read in order to initialize an API and its routes:

```yaml
---
Events:
  HelloWorld:
    Type: Api # More info about API Event Source: https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md#api
    Properties:
      Path: /hello
      Method: get
```

## Packaging and deployment

AWS Lambda Golang runtime requires a flat folder with the executable generated on build step. SAM will use `CodeUri` property to know where to look up for the application:

```yaml
...
    FirstFunction:
        Type: AWS::Serverless::Function
        Properties:
            CodeUri: hello_world/
            ...
```

To deploy your application for the first time, run the following in your shell:

```bash
sam deploy --guided
```

The command will package and deploy your application to AWS, with a series of prompts:

- **Stack Name**: The name of the stack to deploy to CloudFormation. This should be unique to your account and region, and a good starting point would be something matching your project name.
- **AWS Region**: The AWS region you want to deploy your app to.
- **Confirm changes before deploy**: If set to yes, any change sets will be shown to you before execution for manual review. If set to no, the AWS SAM CLI will automatically deploy application changes.
- **Allow SAM CLI IAM role creation**: Many AWS SAM templates, including this example, create AWS IAM roles required for the AWS Lambda function(s) included to access AWS services. By default, these are scoped down to minimum required permissions. To deploy an AWS CloudFormation stack which creates or modifies IAM roles, the `CAPABILITY_IAM` value for `capabilities` must be provided. If permission isn't provided through this prompt, to deploy this example you must explicitly pass `--capabilities CAPABILITY_IAM` to the `sam deploy` command.
- **Save arguments to samconfig.toml**: If set to yes, your choices will be saved to a configuration file inside the project, so that in the future you can just re-run `sam deploy` without parameters to deploy changes to your application.

You can find your API Gateway Endpoint URL in the output values displayed after deployment.

### Testing

We use `testing` package that is built-in in Golang and you can simply run the following command to run our tests locally:

```shell
go test -v ./hello-world/
```

# Appendix

### Golang installation

Please ensure Go 1.x (where 'x' is the latest version) is installed as per the instructions on the official golang website: https://golang.org/doc/install

A quickstart way would be to use Homebrew, chocolatey or your linux package manager.

#### Homebrew (Mac)

Issue the following command from the terminal:

```shell
brew install golang
```

If it's already installed, run the following command to ensure it's the latest version:

```shell
brew update
brew upgrade golang
```

#### Chocolatey (Windows)

Issue the following command from the powershell:

```shell
choco install golang
```

If it's already installed, run the following command to ensure it's the latest version:

```shell
choco upgrade golang
```

## Bringing to the next level

Here are a few ideas that you can use to get more acquainted as to how this overall process works:

- Create an additional API resource (e.g. /hello/{proxy+}) and return the name requested through this new path
- Update unit test to capture that
- Package & Deploy

Next, you can use the following resources to know more about beyond hello world samples and how others structure their Serverless applications:

- [AWS Serverless Application Repository](https://aws.amazon.com/serverless/serverlessrepo/)

```zsh
mashiyun@mikey: ~/private/github.com.main/mashiyun/go-lambda-container/aws/sam (main *%=)
#=>(816) % sam init                                                                                                      <mashiyun> 6:37:18

You can preselect a particular runtime or package type when using the `sam init` experience.
Call `sam init --help` to learn more.

Which template source would you like to use?
        1 - AWS Quick Start Templates
        2 - Custom Template Location
Choice: 1

Choose an AWS Quick Start application template
        1 - Hello World Example
        2 - Multi-step workflow
        3 - Serverless API
        4 - Scheduled task
        5 - Standalone function
        6 - Data processing
        7 - Infrastructure event management
        8 - Machine Learning
Template: 1

 Use the most popular runtime and package type? (Python and zip) [y/N]: N

Which runtime would you like to use?
        1 - dotnet5.0
        2 - dotnetcore3.1
        3 - go1.x
        4 - java11
        5 - java8.al2
        6 - java8
        7 - nodejs14.x
        8 - nodejs12.x
        9 - python3.9
        10 - python3.8
        11 - python3.7
        12 - python3.6
        13 - ruby2.7
Runtime: 3

What package type would you like to use?
        1 - Zip
        2 - Image
Package type: 2

Based on your selections, the only dependency manager available is mod.
We will proceed copying the template using mod.

Project name [sam-app]: testtest

Cloning from https://github.com/aws/aws-sam-cli-app-templates (process may take a moment)

    -----------------------
    Generating application:
    -----------------------
    Name: testtest
    Base Image: amazon/go1.x-base
    Architectures: x86_64
    Dependency Manager: mod
    Output Directory: .
    Next steps can be found in the README file at ./testtest/README.md


    Commands you can use next
    =========================
    [*] Create pipeline: cd testtest && sam pipeline init --bootstrap
    [*] Test Function in the Cloud: sam sync --stack-name {stack-name} --watch
```

```zsh
#=>(859) % sam deploy --guided --profile default                                                                            <mashiyun> 8:05:04

Configuring SAM deploy
======================

        Looking for config file [samconfig.toml] :  Not found

        Setting default arguments for 'sam deploy'
        =========================================
        Stack Name [sam-app]: sam-sample
        AWS Region [us-east-1]: ap-northeast-1
        #Shows you resources changes to be deployed and require a 'Y' to initiate deploy
        Confirm changes before deploy [y/N]: y
        #SAM needs permission to be able to create roles to connect to the resources in your template
        Allow SAM CLI IAM role creation [Y/n]: y
        #Preserves the state of previously provisioned resources when an operation fails
        Disable rollback [y/N]: y
        HelloWorldFunction may not have authorization defined, Is this okay? [y/N]: y
        Save arguments to configuration file [Y/n]: y
        SAM configuration file [samconfig.toml]:
        SAM configuration environment [default]:

        Looking for resources needed for deployment:
        Creating the required resources...
        Successfully created!
         Managed S3 bucket: aws-sam-cli-managed-default-samclisourcebucket-1tj8dunlulih2
         A different default S3 bucket can be set in samconfig.toml
         Image repositories: Not found.
         #Managed repositories will be deleted when their functions are removed from the template and deployed
         Create managed ECR repositories for all functions? [Y/n]: y

        Saved arguments to config file
        Running 'sam deploy' for future deployments will use the parameters saved above.
        The above parameters can be changed by modifying samconfig.toml
        Learn more about samconfig.toml syntax at
        https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-config.html

6f0f51bb1300: Pushed
8d3ac3489996: Pushed
helloworldfunction-5d0338eb8b91-go1.x-v1: digest: sha256:9c4f3496116028164049d8a3664de0c653636ba9d3615c4c9ffadfa9e27d6e9a size: 739

        Deploying with following values
        ===============================
        Stack name                   : sam-sample
        Region                       : ap-northeast-1
        Confirm changeset            : True
        Disable rollback             : True
        Deployment image repository  :
                                       {
                                           "HelloWorldFunction": "262334296846.dkr.ecr.ap-northeast-1.amazonaws.com/samsample1a37ddc0/helloworldfunction19d43fc4repo"
                                       }
        Deployment s3 bucket         : aws-sam-cli-managed-default-samclisourcebucket-1tj8dunlulih2
        Capabilities                 : ["CAPABILITY_IAM"]
        Parameter overrides          : {}
        Signing Profiles             : {}

Initiating deployment
=====================
HelloWorldFunction may not have authorization defined.
Uploading to sam-sample/31b988b185047866febcb74adb634a23.template  1329 / 1329  (100.00%)

Waiting for changeset to be created..

CloudFormation stack changeset
-----------------------------------------------------------------------------------------------------------------------------------------
Operation                          LogicalResourceId                  ResourceType                       Replacement
-----------------------------------------------------------------------------------------------------------------------------------------
+ Add                              HelloWorldFunctionCatchAllPermis   AWS::Lambda::Permission            N/A
                                   sionProd
+ Add                              HelloWorldFunctionRole             AWS::IAM::Role                     N/A
+ Add                              HelloWorldFunction                 AWS::Lambda::Function              N/A
+ Add                              ServerlessRestApiDeploymentd0fc2   AWS::ApiGateway::Deployment        N/A
                                   bb452
+ Add                              ServerlessRestApiProdStage         AWS::ApiGateway::Stage             N/A
+ Add                              ServerlessRestApi                  AWS::ApiGateway::RestApi           N/A
-----------------------------------------------------------------------------------------------------------------------------------------

Changeset created successfully. arn:aws:cloudformation:ap-northeast-1:262334296846:changeSet/samcli-deploy1645690183/9075388f-33d4-4d0b-a100-9700ba9e239b


Previewing CloudFormation changeset before deployment
======================================================
Deploy this changeset? [y/N]: y

2022-02-24 08:10:30 - Waiting for stack create/update to complete

CloudFormation events from stack operations
-----------------------------------------------------------------------------------------------------------------------------------------
ResourceStatus                     ResourceType                       LogicalResourceId                  ResourceStatusReason
-----------------------------------------------------------------------------------------------------------------------------------------
CREATE_IN_PROGRESS                 AWS::IAM::Role                     HelloWorldFunctionRole             -
CREATE_IN_PROGRESS                 AWS::IAM::Role                     HelloWorldFunctionRole             Resource creation Initiated
CREATE_COMPLETE                    AWS::IAM::Role                     HelloWorldFunctionRole             -
CREATE_IN_PROGRESS                 AWS::Lambda::Function              HelloWorldFunction                 -
CREATE_IN_PROGRESS                 AWS::Lambda::Function              HelloWorldFunction                 Resource creation Initiated
CREATE_COMPLETE                    AWS::Lambda::Function              HelloWorldFunction                 -
CREATE_IN_PROGRESS                 AWS::ApiGateway::RestApi           ServerlessRestApi                  -
CREATE_IN_PROGRESS                 AWS::ApiGateway::RestApi           ServerlessRestApi                  Resource creation Initiated
CREATE_COMPLETE                    AWS::ApiGateway::RestApi           ServerlessRestApi                  -
CREATE_IN_PROGRESS                 AWS::Lambda::Permission            HelloWorldFunctionCatchAllPermis   Resource creation Initiated
                                                                      sionProd
CREATE_IN_PROGRESS                 AWS::Lambda::Permission            HelloWorldFunctionCatchAllPermis   -
                                                                      sionProd
CREATE_IN_PROGRESS                 AWS::ApiGateway::Deployment        ServerlessRestApiDeploymentd0fc2   -
                                                                      bb452
CREATE_IN_PROGRESS                 AWS::ApiGateway::Deployment        ServerlessRestApiDeploymentd0fc2   Resource creation Initiated
                                                                      bb452
CREATE_COMPLETE                    AWS::ApiGateway::Deployment        ServerlessRestApiDeploymentd0fc2   -
                                                                      bb452
CREATE_IN_PROGRESS                 AWS::ApiGateway::Stage             ServerlessRestApiProdStage         -
CREATE_COMPLETE                    AWS::ApiGateway::Stage             ServerlessRestApiProdStage         -
CREATE_IN_PROGRESS                 AWS::ApiGateway::Stage             ServerlessRestApiProdStage         Resource creation Initiated
CREATE_COMPLETE                    AWS::Lambda::Permission            HelloWorldFunctionCatchAllPermis   -
                                                                      sionProd
CREATE_COMPLETE                    AWS::CloudFormation::Stack         sam-sample                         -
-----------------------------------------------------------------------------------------------------------------------------------------

CloudFormation outputs from deployed stack
--------------------------------------------------------------------------------------------------------------------------------------------
Outputs
--------------------------------------------------------------------------------------------------------------------------------------------
Key                 HelloWorldFunctionIamRole
Description         Implicit IAM Role created for Hello World function
Value               arn:aws:iam::262334296846:role/sam-sample-HelloWorldFunctionRole-1UGUKT9RNFNGC

Key                 HelloWorldAPI
Description         API Gateway endpoint URL for Prod environment for First Function
Value               https://7d4f53592m.execute-api.ap-northeast-1.amazonaws.com/Prod/hello/

Key                 HelloWorldFunction
Description         First Lambda Function ARN
Value               arn:aws:lambda:ap-northeast-1:262334296846:function:sam-sample-HelloWorldFunction-4uriMee7YWQv
--------------------------------------------------------------------------------------------------------------------------------------------

Successfully created/updated stack - sam-sample in ap-northeast-1
```
