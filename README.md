### Interacting with AWS CLI

### Technological Used:

AWS, Linux

### Project Description:

1- Install and configure AWS CLI tool to connect to our AWS account.

2- Create EC2 Instance using the AWS CLI with all necessary configurations like Security Group.

3- Create SSH key pair.

4- Create IAM resources like User, Group, Policy using the AWS CLI.

5- List and browse AWS resources using the AWS CLI.

### Usage Instructions:

###### Step 1: Install and configure AWS CLI tool to connect to our AWS account.

1-Install AWS CLI on MacOs

```
brew install awscli
```

or

```
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"

sudo installer -pkg AWSCLIV2.pkg -target /
```

2-Configure AWS Cli tool to AWS account via access id and secret access key

```
aws configure
```

###### Step 2: Create EC2 Instance using the AWS CLI with all necessary configurations

1-Create Security Group

```
aws ec2 describe-vpcs
```

```
aws ec2 create-security-group --group-name my-sg --description "my-sq" --vpc-id <vpc_id>
```

```
aws ec2 authorize-security-group-ingress --group-id  <group_id> --protocol tcp --port 22 --cidr <my_current_ip>/network_mask
```

2-Create the key-pair for the EC2 instance

```
aws ec2 create-key-pair --key-name <key_pair_name> --query 'KeyMaterial' --output text > <key_pair_name>.pem
```

```
ls
```

3-Select a subnet for the EC2 instance

```
aws ec2 describe-subnets
```

4-Select a instance type from aws UI or aws cli

```
aws ec2 describe-instance-types
```

5-Select a image from aws UI or aws cli

```
aws ec2 describe-images
```

6-Create EC2 instance

```
aws ec2 run-instances \
> --image-id ami-0692dea0a2f8a1b35 \
> --count 1 \
> --instance-type t2.micro \
>  --key-name MyKpCli \
> --security-group-ids sg-0fb6e2049ed1053fa \
> --subnet-id subnet-0b5df23ba8e8377da
```

###### Step 3: Login EC2 Instance via SSH

1-Move key_pair file from current directory to a new directory under .ssh

```
mv MyKpCli.pem ~/.ssh/
```

2-Change the permissions of the .pem file to least privileges

```
sudo chmod 400 ~/.ssh/MyKpCli.pem
```

3-Get the public ip address of the created EC2 instance

```
aws ec2 describe-instances
```

4-login into EC2 instance

```
ssh -i .ssh/MyKpCli.pem ec2-user@52.62.76.47
```

```
whoami
```

###### Step 4: Create IAM resources like User, Group, Policy using the AWS CLI.

1-List all users under the current aws account

```
aws iam list-users
```

2-List all groups under the current aws account

```
aws iam list-groups
```

3-Create a new group under the current aws account

```
aws iam create-group --group-name mygroup
```

4-Create a new user under the current aws account

```
aws iam create-user --group-name myuser
```

5-Add a new user into a group

```
aws iam add-user-to-group --user-name myuser --group-name mygroup
```

6-Check the account ID of a user

```
aws iam get-user --user-name myuser
```

7-Check the details of a group

```
aws iam get-group --group-name mygroup
8-Assign a policy to a group by attached policy arn (arn stands for amazon resource name, which is a unique id)
```

aws iam attach-group-policy --group-name mygroup --policy-arn arn:aws:iam::aws:policy/AmazonEC2FullAccess

```
9-Check the policies of a group
```

aws iam list-attached-group-policies --group-name mygroup

```
10-Get a policy arn by policy name
```

aws iam list-policies --query 'Policies[?PolicyName==`AmazonEC2FullAccess`].{ARN:arn}` --output text

```
###### Step 5: Create the password, access ID and secret access key for a new user
1-Create the password for a new user
```

aws iam create-login-profile --user-name myuser --password {pwd} --password-reset-required

```
2-Create the access ID and secret access key for a new user
```

```
aws iam create-access-key --user-name myuser
```

###### Step 6: Create a new policy

1-Create a json file under the current user folder via vim and paste the policy into the json file

```
vim changePasswordPolicy.json
```

![image](image/Screenshot%202023-02-28%20at%203.06.54%20pm.png)

```
aws iam create-policy --policy-name changePasswordPolicy --policy-document file://changePasswordPolicy.json
```
