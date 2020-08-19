
Create:

- VPC
- Public Subnet 1a
- Public Subnet 1b
- Private Subnet 1a
- Private Subnet 1b
- Intenet Gateway
- Nat gateway 1a
- Nat gateway 1b
- Nat EIP Allocation 1a
- Nat EIP Allocation 1b
- LaodBalancer External
- LaodBalancer HTTP HTTP Listener
- Internal LoadBalancer
- Global Accelerator
- Lambda turnOff turnOn EC Instances

## Cloudformation cli create stack

```bash
aws cloudformation create-stack \
--region sa-east-1 \
--stack-name <NAME> \
--template-body file://cf-jaf.yml \
--parameters file://parameters.json \
--capabilities CAPABILITY_NAMED_IAM 
```

## Cloudformation cli change set

```bash
#!/bin/bash

aws s3 cp --recursive ../<FOLDER> s3://<TEMPLATE BUCKET>

changeSetName=$(cat /dev/urandom | tr -dc 'a-zA-Z' | fold -w 16 | head -n 1)
echo "Change set name: $changeSetName"

aws cloudformation create-change-set \
--region sa-east-1 \
--stack-name <NAME> \
--template-body file://cf-jaf.yml \
--parameters file://parameters.json \
--capabilities CAPABILITY_NAMED_IAM \
--change-set-name $changeSetName

aws cloudformation wait change-set-create-complete \
--region sa-east-1 \
--stack-name <NAME> \
--change-set-name $changeSetName

aws cloudformation execute-change-set \
--region sa-east-1 \
--stack-name <NAME> \
--change-set-name $changeSetName

aws cloudformation wait  stack-update-complete \
--region sa-east-1 \
--stack-name <NAME>
```
