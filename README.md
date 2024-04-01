# Localstack 3.2 | Elastic Compute Cloud (EC2)
Localtack 3.2 service ec2 docker compose.

**Overview.** <br />
LocalStack is a cloud service emulator that runs in a single container on your laptop or in your CI environment. With LocalStack, you can run your AWS applications or Lambdas entirely on your local machine without connecting to a remote cloud provider!

&nbsp;

<div align="center">
    <img src="./gambar-petunjuk/ss_dockerhub_localstack.png" alt="ss_dockerhub_localstack" style="display: block; margin: 0 auto;">
    <p align="center">URL : https://hub.docker.com/layers/localstack/localstack/3.2/images/sha256-e123bfa05e504ab5fd3eac4c2209f375aff0fbc9401184c4ea34d0b7b919a7af?context=explore</p>
</div> 

&nbsp;

---

ATTENTION !<br />

Localstack `EC2 instances` will have actual capabilities only with the Localstack Pro service.
<div align="center">
    <img src="./gambar-petunjuk/ss_ec2_mocks_localstack.png" alt="ss_ec2_mocks_localstack" style="display: block; margin: 0 auto;">
    <p align="center">URL : https://github.com/localstack/localstack/issues/9470</p>
</div> 

---

&nbsp;

&#x1F516; Information on the user's device.<br />

<pre>
    ❯ system_profiler SPSoftwareDataType SPHardwareDataType

        Software:
            System Software Overview:
                System Version: macOS 13.3.1 (22E261)
                Kernel Version: Darwin 22.4.0
                Boot Volume: Macintosh HD
                Boot Mode: Normal    
                . . .

        Hardware:
            Hardware Overview:
                Model Name: MacBook Pro
                Model Identifier: MacBookPro17,1
                Model Number: MYD82ID/A
                Chip: Apple M1
                Total Number of Cores: 8 (4 performance and 4 efficiency)
                Memory: 8 GB
                . . .
</pre>

&nbsp;

**&#x1F536; Reference :**<br />

<pre>
    <b>[ Localstack ]</b><br />
    - User Guides | Elastic Compute Cloud (EC2)
      https://docs.localstack.cloud/user-guide/aws/ec2/
</pre>

&nbsp;

---

&nbsp;

## &#x1F530; Start Deployment in docker compose.

<pre>
    ❯ vim docker-compose.yml

        version: '3.7'
        
        services:
          localstack:
            image: localstack/localstack:3.2
            container_name: localstack
            environment:
              - DOCKER_HOST=unix:///var/run/docker.sock
              - EDGE_PORT=4566
              - SERVICES=lambda,s3, ec2
              - DEBUG=1
            ports:
              - "4566:4566"
            volumes:
              - ./localstack/localstack_root:/var/lib/localstack  
              - ./localstack/localstack_home:/home/localstack
              - "/var/run/docker.sock:/var/run/docker.sock"
        networks:
          default:
            name: localstack
</pre>

---

&nbsp;

Run.
<pre>
    ❯ docker-compose up

        [+] Running 2/2
        ⠿ Network localstack    Created                                                                                                                                                                 0.0s
        ⠿ Container localstack  Created                                                                                                                                                                 0.0s
        Attaching to localstack
        localstack  | 
        localstack  | LocalStack version: 3.2.0
        localstack  | LocalStack Docker container id: af2879bb5e1b
        localstack  | LocalStack build date: 2024-02-28
        localstack  | LocalStack build git hash: 4a4692dd5
        localstack  | 
        localstack  | 2024-03-31T21:26:00.107  WARN --- [  MainThread] localstack.deprecations    : EDGE_PORT is deprecated (since 2.0.0) and will be removed in upcoming releases of LocalStack! This configuration will be migrated to GATEWAY_LISTEN
        localstack  | 2024-03-31T21:26:00.218  INFO --- [-functhread4] hypercorn.error            : Running on https://0.0.0.0:4566 (CTRL + C to quit)
        localstack  | 2024-03-31T21:26:00.218  INFO --- [-functhread4] hypercorn.error            : Running on https://0.0.0.0:4566 (CTRL + C to quit)
        localstack  | Ready.

        localstack  | 2024-04-01T03:31:09.609  INFO --- [   asgi_gw_0] localstack.request.aws     : AWS ec2.CreateKeyPair => 200
        localstack  | 2024-04-01T03:32:30.716  INFO --- [   asgi_gw_0] localstack.request.aws     : AWS ec2.AuthorizeSecurityGroupIngress => 200
        localstack  | 2024-04-01T03:41:34.522  INFO --- [   asgi_gw_0] localstack.request.aws     : AWS ec2.DescribeSecurityGroups => 200
        localstack  | 2024-04-01T04:14:51.762  INFO --- [   asgi_gw_0] localstack.request.aws     : AWS ec2.RunInstances => 200
</pre>

&nbsp;

Check.<br />
[localstack.http]
<pre>
    GET http://localhost:4566/_localstack/health HTTP/1.1
</pre>
Response.
<pre>
    HTTP/1.1 200 
    Content-Type: application/json
    Content-Length: 889
    date: Sun, 31 Mar 2024 21:28:21 GMT
    server: hypercorn-h11
    Connection: close

    {
    "services": {
        "acm": "disabled",
        "apigateway": "disabled",
        "cloudformation": "disabled",
        "cloudwatch": "disabled",
        "config": "disabled",
        "dynamodb": "disabled",
        "dynamodbstreams": "disabled",
        "ec2": "disabled",
        "es": "disabled",
        "events": "disabled",
        "firehose": "disabled",
        "iam": "disabled",
        "kinesis": "disabled",
        "kms": "disabled",
        "lambda": "available",
        "logs": "disabled",
        "opensearch": "disabled",
        "redshift": "disabled",
        "resource-groups": "disabled",
        "resourcegroupstaggingapi": "disabled",
        "route53": "disabled",
        "route53resolver": "disabled",
        "s3": "running",
        "s3control": "disabled",
        "scheduler": "disabled",
        "secretsmanager": "disabled",
        "ses": "disabled",
        "sns": "disabled",
        "sqs": "available",
        "ssm": "disabled",
        "stepfunctions": "disabled",
        "sts": "available",
        "support": "disabled",
        "swf": "disabled",
        "transcribe": "disabled"
    },
    "edition": "community",
    "version": "3.2.0"
    }
</pre>

&nbsp;

&nbsp;

&nbsp;

---

&nbsp;

## &#x1F530; Elastic Compute Cloud (EC2)
**Get started with Amazon Elastic Compute Cloud (Amazon EC2) on LocalStack**<br />

&nbsp;

**Create a Key Pair**<br />
To create a key pair, you can use the CreateKeyPair API. Run the following command to create the key pair and pipe the output to a file named key.pem:

<pre>
    ❯ docker exec -it localstack /bin/bash
</pre>

<pre>
    ❯ awslocal ec2 create-key-pair --key-name my-key --query 'KeyMaterial' --output text | tee key.pem


            -----BEGIN RSA PRIVATE KEY-----
            MIIEpAIBAAKCAQEA8457nyaxPljw/mj1pRP/aD1ArnLG+pE8zHxpDq2lzw57FXwN
            4DLqyiQS3/QhXAmlZaZybC9y9AUB6lYtEmsaNvkUoWTxK6U8SZV25t7VngcWbgpJ
            cTGg4SJFxYPZ+cX0ok8V7dUhXDQu0lskias2+98Ipl9egckdYweCZx9G8nd5DNnJ
            tgjVEqrmxsdgO/64CA3y+YgebOsysm5j3L3L4K86v8kSsk2u0DbGcXdtjk311hRM
            JrbrKKQISHCyHm1VlrnPcRChzVpYdkSRYKI+4EvplqDG8fAi/oiCafs6BFqNz9bn
            iXUbvoEWyDPrLv5zKKs56HAPVbdMoFXjP1fE0wIDAQABAoIBAGZNEUjioC0/f45k
            +NULZsrae5oqtMBXk/GSSjBzqMMlYna+QjfLO0qHx3PRH9gAZzwgo0wkzASKO+k5
            pDnpybuQeOVnuFMsVvvTb3t+2rxDXtz+riWBAoG9+w+BF+QtjVlFncDltlr7wjTy
            OpEm3PQDlScIxPH/zzui0lfNT+gUi/rrenVxPFk9oAsLt3+YxASp9xY9oE4nFNDk
            SlthUuRpcEU2T0dGZbzU61W2EeKIDiqR6Crm1jYyQtL95XKXJrdqh4grUaA9Z1XX
            1U09AA3+X1Z//Vrnvhz3phVu74Y6ZGINDsE/AfODG85+fFWVvP0BMooH8WA9Wgu9
            K7APhmkCgYEA+1vahJH+zry8W8CAK8D/TI1l4Q4+pmyVdfICI8/dyktjaGobK/GL
            9rCkitr/3kNxSQVDH8Y0ndJ+qTFkkxlCEazM1rvY4AVDKvYhLFVluFouSgKj6Rhk
            zuTrcclA1GRebscFU8XNSBtS1nhSs9FOhm1QvXWUc1V36ar8GtN1EusCgYEA+A2/
            vozft6CKJP/kNCNaosH7PrgLDFIyh1HoZTviP+KL5T6RjkKPwrkTptmv0pSDI7Us
            OuQendgwmvukugZLGas5ZJ28ui7hf6cQzGV+JF+7RAN2GGqLshiCUqtncqg2FydF
            eRbzjGVswu5pGz4kQoGXOjCvcz9hu5DjQMqHC7kCgYEA98bM+l/cWTjtSFjTP85J
            G9JKunZLRczF7HU6rMisbkywWm42CLRb7zqjiIlnLlc3Je79AyZkGas01l3tMZ/1
            Y+z+IzMbD4HAe2oSu1wXIIotFSHTJ+S3AsfgW9Myh+vEttiTJMhYmprspqQHimBq
            UtMRgyGTy7lVsk6to1gNES8CgYEAkLiU0jummp/TeVrCbZji3GqIh0MhTwL17/Vd
            vRJ/If6u2AT1LyaucVFBoesHpbh3+nFNaN6G7lifowyGQvJBBqzbQ1S0M3v+nFeA
            eYANZHNl8nyCfiRLdJDQGCNgq4hwZnnHEqrNVXAnUGOAdyB+Tz8EWDLnajnkb2ZM
            8BQ5TiECgYAaOTEy5jXWWztykSpqfiWGAoBdM+ktkENfnJblEH8cuNbDrA1Z9dhz
            Lr8HO3EkR+SyqTe/hRrFo+B3kfHpmzYLAlOYYkCDXjnLoqUmB5iXE6VFxf7EAjjq
            DVJDfQELikzVrBPTePrzLbKWMjybEyrcbhP9VgW3+z0uiP68bsqUFA==
            -----END RSA PRIVATE KEY-----
</pre>

<pre>
    ❯ ls -lah

        -rw-r--r-- 1 root root 1.7K Apr  1 02:30 key.pem
</pre>

&nbsp;

You can assign necessary permissions to the key pair file using the following command:
<pre>
    ❯ chmod 400 key.pem
</pre>

<pre>
    ❯ ls -lah

        -r-------- 1 root root 1.7K Apr  1 02:30 key.pem
</pre>

&nbsp;

Alternatively, we can import an existing key pair, for example if you have an SSH public key in your home directory under ~/.ssh/id_rsa.pub:
<pre>
    ❯ awslocal ec2 import-key-pair --key-name my-key --public-key-material file://~/.ssh/id_rsa.pub
</pre>

Current ssh public-key on the host device.
<pre>
    ❯ ccat ~/.ssh/id_rsa.pub

        ssh-rsa AAAAB3NzaC1yc2.....31Nz6xrynS+sKBc5/IZFM= ...@...-MacBook-Pro-7.local
</pre>

&nbsp;

**Add rules to your security group**<br />
Currently, LocalStack only supports the default security group. You can add rules to the security group using the AuthorizeSecurityGroupIngress API. Run the following command to add a rule to allow inbound traffic on port 8000:
<pre>
    ❯ awslocal ec2 authorize-security-group-ingress --group-id default --protocol tcp --port 8000 --cidr 0.0.0.0/0

        {
            "Return": true,
            "SecurityGroupRules": [
                {
                    "SecurityGroupRuleId": "sgr-5097b9a95dfd554e2",
                    "GroupId": "sg-3cd65e02af566a5a9",
                    "GroupOwnerId": "000000000000",
                    "IsEgress": false,
                    "IpProtocol": "tcp",
                    "FromPort": 8000,
                    "ToPort": 8000,
                    "CidrIpv4": "0.0.0.0/0",
                    "Description": ""
                }
            ]
        }

</pre>

The above command will enable rules in the security group to allow incoming traffic from your local machine on port 8000 of an emulated EC2 instance.

&nbsp;

**Run an EC2 instance**<br />
You can fetch the Security Group ID using the DescribeSecurityGroups API. Run the following command to fetch the Security Group ID:

Install jq (a lightweight and flexible command-line JSON processor).
<pre>
    # Install jq
    ❯ apt update && apt install jq
    ❯ jq --version
        jq-1.6
</pre>
<pre>
    ❯ sg_id=$(awslocal ec2 describe-security-groups | jq -r '.SecurityGroups[0].GroupId')

    ❯ echo $sg_id
        sg-3cd65e02af566a5a9
</pre>

&nbsp;

To start your Python Web Server in your locally emulated EC2 instance, you can use the following user script by saving it to a file named user_script.sh:
<pre>
    ❯ touch ./localstack/localstack_home/user_script.sh

    ❯ vim ./localstack/localstack_home/user_script.sh

        ...
        #!/bin/bash -xeu

        apt update
        apt install python3 -y
        python3 -m http.server 8000
</pre>

You can now run an EC2 instance using the RunInstances API. Run the following command to run an EC2 instance by adding the appropriate Security Group ID that we fetched in the previous step:
<pre>
    ❯ pwd
        /home/localstack
    
    ❯ chmod +x user_script.sh

    ❯ ls -lah
        -r-------- 1 root root 1.7K Apr  1 02:30 key.pem
        -rwxr-xr-x 1 root root   80 Apr  1 03:51 user_script.sh

    &lt;!-- awslocal ec2 run-instances \
        --image-id ami-ff0fea8310f3 \
        --count 1 \
        --instance-type t3.nano \
        --key-name my-key \
        --security-group-ids '&lt;SECURITY_GROUP_ID&gt;' \
        --user-data file://./user_script.sh --&gt;

</pre>
<pre>
    ❯ awslocal ec2 run-instances --image-id ami-ff0fea8310f3 --count 1 --instance-type t3.nano --key-name my-key --security-group-ids $sg_id --user-data file://./user_script.sh
</pre>
<pre>
            {
                "Groups": [
                    {
                        "GroupName": "default",
                        "GroupId": "sg-245f6a01"
                    }
                ],
                "Instances": [
                    {
                        "AmiLaunchIndex": 0,
                        "ImageId": "ami-ff0fea8310f3",
                        "InstanceId": "i-edc0beeaeae9b1a18",
                        "InstanceType": "t3.nano",
                        "KernelId": "None",
                        "KeyName": "my-key",
                        "LaunchTime": "2024-04-01T04:14:51Z",
                        "Monitoring": {
                            "State": "disabled"
                        },
                        "Placement": {
                            "AvailabilityZone": "us-east-1a",
                            "GroupName": "",
                            "Tenancy": "default"
                        },
                        "PrivateDnsName": "ip-10-105-252-21.ec2.internal",
                        "PrivateIpAddress": "10.105.252.21",
                        "PublicDnsName": "ec2-54-214-147-31.compute-1.amazonaws.com",
                        "PublicIpAddress": "54.214.147.31",
                        "State": {
                            "Code": 0,
                            "Name": "pending"
                        },
                        "StateTransitionReason": "",
                        "SubnetId": "subnet-f7bd8683",
                        "VpcId": "vpc-2e8cff39",
                        "Architecture": "x86_64",
                        "BlockDeviceMappings": [
                            {
                                "DeviceName": "/dev/sda1",
                                "Ebs": {
                                    "AttachTime": "2024-04-01T04:14:51Z",
                                    "DeleteOnTermination": false,
                                    "Status": "in-use",
                                    "VolumeId": "vol-30f43963"
                                }
                            }
                        ],
                        "ClientToken": "ABCDE0000000000003",
                        "EbsOptimized": false,
                        "Hypervisor": "xen",
                        "NetworkInterfaces": [
                            {
                                "Association": {
                                    "IpOwnerId": "000000000000",
                                    "PublicIp": "54.214.147.31"
                                },
                                "Attachment": {
                                    "AttachTime": "2015-01-01T00:00:00Z",
                                    "AttachmentId": "eni-attach-069da715",
                                    "DeleteOnTermination": true,
                                    "DeviceIndex": 0,
                                    "Status": "attached"
                                },
                                "Description": "Primary network interface",
                                "Groups": [
                                    {
                                        "GroupName": "default",
                                        "GroupId": "sg-3cd65e02af566a5a9"
                                    }
                                ],
                                "MacAddress": "1b:2b:3c:4d:5e:6f",
                                "NetworkInterfaceId": "eni-39ee55b2",
                                "OwnerId": "000000000000",
                                "PrivateIpAddress": "10.105.252.21",
                                "PrivateIpAddresses": [
                                    {
                                        "Association": {
                                            "IpOwnerId": "000000000000",
                                            "PublicIp": "54.214.147.31"
                                        },
                                        "Primary": true,
                                        "PrivateIpAddress": "10.105.252.21"
                                    }
                                ],
                                "SourceDestCheck": true,
                                "Status": "in-use",
                                "SubnetId": "subnet-f7bd8683",
                                "VpcId": "vpc-2e8cff39"
                            }
                        ],
                        "RootDeviceName": "/dev/sda1",
                        "RootDeviceType": "ebs",
                        "SecurityGroups": [
                            {
                                "GroupName": "default",
                                "GroupId": "sg-3cd65e02af566a5a9"
                            }
                        ],
                        "SourceDestCheck": true,
                        "StateReason": {
                            "Code": "",
                            "Message": ""
                        },
                        "VirtualizationType": "paravirtual"
                    }
                ],
                "OwnerId": "000000000000",
                "ReservationId": "r-745f983a"
            }
</pre>

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

---

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;
