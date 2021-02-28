# Setup Hit and Run AWS EC2 Ubuntu instance - all from the CLI

https://github.com/kurtzace/Diary2021/issues/4
   
   
Laptop OS: Ubuntu  20.04 


## Setup AWS CLI
Follow steps from https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html

Open terminal:
```
laptopUser@laptop>:sudo apt  install awscli 
laptopUser@laptop>:aws configure
```
Apply secrets

Verify secrets

```
laptopUser@laptop>:cd ~/.aws
laptopUser@laptop>:~/.aws$ ls
config  credentials
laptopUser@laptop>:~/.aws$ cat credentials 

 
 
 laptopUser@laptop>:cd ~/development
 laptopUser@laptop>:mkdir aws


laptopUser@laptop>:~/development/aws$ aws ec2 create-key-pair --key-name CompanyKeyPairFeb2021 --region ap-south-1 --query 'KeyMaterial' --output text > CompanyKeyPairFeb2021.pem
laptopUser@laptop>:~/development/aws$ ls
CompanyKeyPairFeb2021.pem  

```
## Collect info for EC2 creation
Before you run instructions from 
https://docs.aws.amazon.com/cli/latest/reference/ec2/run-instances.html



Region guide https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-available-regions
```
 aws ec2 --region ap-south-1 describe-vpcs
{
    "Vpcs": [
        {
 ....
            "VpcId": "vpc-id"
...
```

Note vpc id

We will use the following command
```
laptopUser@laptop>:~/development/aws$ aws ec2 describe-key-pairs --region ap-south-1
{
    "KeyPairs": [
        {
            "KeyPairId": "key-id",
            "KeyFingerprint": "...",
            "KeyName": "CompanyKeyPairFeb2021",
            "Tags": []
        }
    ]
}
```

```
laptopUser@laptop>:~/development/aws$ aws ec2 --region ap-south-1 describe-security-groups
{

"GroupId": "sg-id",
```


find out ap-south-1 ubuntu 20.04 LTS from either

```
aws ec2 describe-images --output text --region ap-south-1 --filters "Name=name,Values=ubuntu*bionic*"

```

Contents of mapping.json. This example adds /dev/sda1 an empty EBS volume with a size of 75 GiB.    

```
[
    {
        "DeviceName": "/dev/sda1",
        "Ebs": {
            "VolumeSize": 75
        }
    }
]
```

## Create instance
```
laptopUser@laptop>:aws ec2 run-instances \
    --region ap-south-1 \
    --image-id ami-073c8c0760395aab8  \
    --instance-type t2.large \
    --security-group-ids sg-sgid \
    --associate-public-ip-address \
    --key-name CompanyKeyPairFeb2021 --block-device-mappings file://mapping.json
    
```

## Get info like IP and instance id
```
laptopUser@laptop>:aws ec2 describe-instances --region ap-south-1
{
    "Reservations": [
        {
            "Groups": [],
            "Instances": [
                {
                    "AmiLaunchIndex": 0,
                    "ImageId": "ami-073c8c0760395aab8",
                    "InstanceId": "i-id",


```
 "PublicDnsName": "blahblah.ap-south-1.compute.amazonaws.com",

Note even public ip too


## Allow SSH
```
laptopUser@laptop>:~/development/aws$ curl https://checkip.amazonaws.com
xxx.xx.xx.ip
laptopUser@laptop>:~/development/aws$ aws ec2 authorize-security-group-ingress --region ap-south-1 --group-id sg-sgid --protocol tcp --port 22 --cidr xxx.xx.xx.ip/32
```


## Allow few websites access   

```
aws ec2 authorize-security-group-egress --group-id sg-sgid --ip-permissions IpProtocol=tcp,FromPort=8000,ToPort=9000,IpRanges='[{CidrIp=xxx.xx.xx.ip/32}]' --region ap-south-1
```

## Get into the machine
```
laptopUser@laptop>:ssh -i "CompanyKeyPairFeb2021.pem" ubuntu@xx.xx.pub.lic.ipOfInstance
```
Install docker material

```
awscli>curl -fsSL https://get.docker.com -o get-docker.sh
awscli>sudo apt install curl
awscli>sudo sh get-docker.sh
awscli>curl -fsSL https://get.docker.com -o get-docker.sh
awscli>sudo apt install curl
awscli>sudo apt-get update
awscli>sudo apt install curl
awscli>curl -fsSL https://get.docker.com -o get-docker.sh
awscli>sudo sh get-docker.sh
awscli>sudo usermod -aG docker ubuntu
awscli>sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
awscli>sudo apt install git
awscli>exit

```

## from laptop
```

aws ec2 stop-instances --region ap-south-1 --instance-id i-myInstanceId

aws ec2 start-instances --region ap-south-1 --instance-id i-myInstanceId

```


all ec2 commands reference
https://docs.aws.amazon.com/cli/latest/reference/ec2/index.html#cli-aws-ec2


## Appendix 

some reference


Aws Cli

https://youtu.be/sIx7MO4rcCU

https://youtu.be/8LfzayKQ3oo

![A88A9202-6FAF-46B3-BC80-398D9C842192](https://user-images.githubusercontent.com/2136211/109416499-a6085380-79e4-11eb-8349-934828199d85.jpeg)
![5FCC5CE4-C604-43A5-8889-6E0E639EDCA1](https://user-images.githubusercontent.com/2136211/109416502-aacd0780-79e4-11eb-846a-35a61a7fdd37.png)
![2074CB5F-9400-459F-946F-B7A8BEE4AFBF](https://user-images.githubusercontent.com/2136211/109416504-ad2f6180-79e4-11eb-9571-db1b10690522.png)
![386E5C3B-067E-44B9-9FF5-1D05F8452F27](https://user-images.githubusercontent.com/2136211/109416505-aef92500-79e4-11eb-81da-fc1b5062e9f0.jpeg)
![8619A7CB-1A20-4065-9311-853D2815AAA4](https://user-images.githubusercontent.com/2136211/109416506-af91bb80-79e4-11eb-9bff-1dabf585083d.jpeg)
![55407940-BAA3-48B6-BAF8-B6A5BD17FBA1](https://user-images.githubusercontent.com/2136211/109416507-af91bb80-79e4-11eb-82ba-4c454f084486.jpeg)
![7D87E59A-A868-4124-ABAB-802EF62C70D1](https://user-images.githubusercontent.com/2136211/109416508-b02a5200-79e4-11eb-8c13-e3c5738c8954.jpeg)


