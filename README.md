# Cloud Formation - VPC, ELB, and more

* VPC​ ​with​ ​two​ ​public​ ​and​ ​private​ ​subnets
    * Route​ ​tables​
* Security​ ​Group​ ​to​ ​allow​ ​port​ ​80​ ​and​ ​443
* Balancers:
    * ELB​ ​
    * ALB
        * Listener
        * Target Group
* Private​ ​route53​ ​hosted​ ​zone​ ​and​ Route 53 Alias ​entry​ ​for​ ​both​ ​ALB​ ​and​ ​ELB
    * Additional  public hosted zone
* IAM​ ​Policy​

### General

Use **vpc.yaml** cloudformation template to create all the resources 

### Prerequisites

* aws config should be properly configured

```
aws configure --profile XXX
...
export AWS_PROFILE=XXX
```

* aws write permissions for **cloudformation, iam, ec2, elb, elasticloadbalancing** services are required

### Required parameters

```
env_name=XXX
```

### Examples

Set params:
```
template_url="file:////tmp/cloud-formation/vpc.yaml"
stack_name=challenge-v03
env_name=challenge
```
Validate template
```
aws cloudformation validate-template --template-body "${template_url}"
```
Create CF stack
```
aws --profile alex-lab cloudformation create-stack \
    --capabilities CAPABILITY_NAMED_IAM \
    --disable-rollback \
    --stack-name "${stack_name}" \
    --template-body "${template_url}" \
    --parameters ParameterKey=EnvironmentName,ParameterValue="${env_name}",UsePreviousValue=true \
    --tags Key=Environment,Value="\"${env_name}\"" \
           Key=Name,Value="\"${stack_name}\""
```
Optional - Update CF stack
```
    aws --profile alex-lab cloudformation update-stack \
    --capabilities CAPABILITY_NAMED_IAM \
    --stack-name "${stack_name}" \
    --template-body "${template_url}" \
    --parameters ParameterKey=EnvironmentName,ParameterValue="${env_name}"

```

