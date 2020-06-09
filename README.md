# ROBOfactory-node-catalogue-service-infra
ROBOfactory node catalogue service infra

## Deploy infra pipeline
NOTE: make sure `secretsmanager:my-app-secrets:SecretString:git-personal-access-token`
is availabe in the account.
```
# Create\update
aws cloudformation deploy \
--template-file pipeline.yml \
--stack-name node-robot-catalogue-service-infra-pipeline \
--parameter-overrides \
GitHubOwner=Manoj2087 \
BranchName=master \
MyAppBuildSpecPath=buildspec.yml \
RepositoryName=ROBOfactory-node-catalogue-service-infra \
--capabilities CAPABILITY_IAM \
--region ap-southeast-2

# Delete
aws cloudformation delete-stack \
--stack-name node-robot-catalogue-service-infra-pipeline \
--region ap-southeast-2
```

## Deploy infra
NOTE: make sure EC2KeyPair is availabe in the account.
```
# Create\update
aws cloudformation deploy \
--template-file cloud-formation/infra.yml \
--stack-name node-robot-catalogue-service-infra \
--parameter-overrides \
ProjectName=robofactory \
CidrPrefix=10.10 \
EnvName=test \
InstanceType=t3.large \
EC2KeyPair=DevOpsPro \
DesiredCapacity=2 \
MaxSize=6 \
--capabilities CAPABILITY_IAM \
--tags \
Name=node-robot-catalogue-service-infra \
Env=test \
Build=1234 \
--region ap-southeast-2

# Delete
aws cloudformation delete-stack \
--stack-name node-robot-catalogue-service-infra \
--region ap-southeast-2
```