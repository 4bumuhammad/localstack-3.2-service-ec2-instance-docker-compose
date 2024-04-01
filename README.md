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
    <img src="./gambar-petunjuk/ss_ec2_mocks_localstack_rev1.png" alt="ss_ec2_mocks_localstack" style="display: block; margin: 0 auto;">
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
    Content-Length: 890
    date: Mon, 01 Apr 2024 08:02:17 GMT
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
        "ec2": "running",
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
        "s3": "available",
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

**Create a Key Pair.**<br /><br />
To create a key pair, you can use the CreateKeyPair API. Run the following command to create the key pair and pipe the output to a file named key.pem:

<pre>
    ❯ docker exec -it localstack /bin/bash
</pre>

<pre>
    ❯ awslocal ec2 create-key-pair --key-name my-key --query 'KeyMaterial' --output text | tee key.pem


            -----BEGIN RSA PRIVATE KEY-----
            MIIEowIBAAKCAQEA6dubR7/q3egcMwSgxqJxy0tSJmCZ3iPzgEXJp/+F7sXAlBpY
            navR0/4ZKCvcEQzrcoRUAQ+ekDmXswgXzly0y+JLV0NaSpwWiCJxD7gbGvsgLNUN
            T3139RnYc1ePd8mAP5IlLPuvLqSfOsxS5Gz7wBDyII4dFq4goCHqO6l+NzQIxB3+
            /BqHLkUq+ifwenM8TwHc8obKSkSdqG190ZjgZRqek9Zray5ArN6sl0yfq6q5OIcI
            nueId6sXdm9Li0y4t95Q1BKZ7kkWVaL5fD+M3XdEAtpn3qY7iw4KmrnFNgngeUZX
            9ghTIEPn8ZaC0oCky0rfziUd7QAs11OiVKVyGwIDAQABAoIBAHKwISigbeJTPVUl
            7WVM8GrHbyvkD0HatsYPM7UAwyHU+LuJajE1PQvu306Mp/nysJtyWzJEAzrvC78g
            9m/dlksxGUrb/LF/eCteXL1dSGnEg8SI3eTAfy6EdNsoM3OHLi6/ER1M9150HKT8
            5KAi9Jn+lYgyugBP+C8gQtvspToTikS6THkKUygut9nN9yAj36sGhZvbbypX9amA
            OAImEkEu9hLZQL/o/nh/7GtM/GognDopsorc0QNxz4+zDZLc1L034F+tgCQfssvP
            B6nHpgodz53oPRogz9/6gbPSe4QdGbYHZHJ1ywSvshY0lqu6djQicLx2YW5DVGVd
            xro+DrUCgYEA9RaozTBkJYxm8PrDjD6hfKnXX/KzxP8a0SLuV1rxAmhUXY51FKTt
            gqcd0jsSKsVqNLe2gIw1xrB4wskxLANPzKTZbVj1HQeVL0SZ5ltdLW3d0aydRQv3
            uKXsCS3ngkM02Vbyxa0n4CYV/C/JwPOrf6pO2EBzD404fuFAq9QY2bcCgYEA9ETy
            t8C8/J6nub+gJ1kgbLnemrjGoYJufkX5/abY9dpCnzN2mc+P5EdPcpH3FS3FEp//
            jjCPc9qqHuqsCtjBDmMiaQWr/d2vAjw2FdeRU3py7xFGVuxckzWHXv3b6pqK+G37
            QFuJcoHq7cr6ZuImI1HsqXmvuzCyVweDGxPQ+r0CgYBO5IRhyHn0PEXGvHHF60NA
            Rcu2bwG/H/8Z4wylwfhitgKyOvElPBHYRMMssScf7XxKi/iLWJbW5md5DBpv+IDv
            dwEmWcRNG+vB02tW44IS5ZLJs67MUxXiskZQjh4/qDKS45IyX5K+bIQXMarYGZfS
            +Jd4tQZUszjEXu3aYeO98QKBgQDAWXbj+dMX/ntHSr17nrRoyaigH7bSQdpudv4X
            noYkez2RDb5r+kVt1jWam5R8yfDw7KsZteWQrnTeKZ9WXz8O7EORfdTk8Oc6FF3m
            rzySBs9wsCK6mh+AEFvMEo1VXWLno/uAv1QppmJv5DFwSM8Naxq4jcky13LoE1NL
            RDyA5QKBgGWE7eIuq/R49/ewxrL7RKIXPxZWJNYTG6AzjmZrDy+ZLEZAJ8EdTcC4
            gxNIYfxvXcW29wBZtsnRoKyFdnO4O3xbiKJqjeU9aptcIVbYSblsFlk0ExyjRsfl
            hKT9nkWWRdI/ZGKeJV+H/j/VjrqrRclLSacCFnJO7jIH7ekucU4K
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

**Add rules to your security group.**<br /><br />
Currently, LocalStack only supports the default security group. You can add rules to the security group using the AuthorizeSecurityGroupIngress API. Run the following command to add a rule to allow inbound traffic on port 8000:
<pre>
    ❯ awslocal ec2 authorize-security-group-ingress --group-id default --protocol tcp --port 8000 --cidr 0.0.0.0/0

        {
            "Return": true,
            "SecurityGroupRules": [
                {
                    "SecurityGroupRuleId": "sgr-adfb8268efbfbae34",
                    "GroupId": "sg-6db4f403b96d01afd",
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

**Run an EC2 instance.**<br /><br />
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
        sg-6db4f403b96d01afd
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

    # or

    ❯ awslocal ec2 run-instances --image-id ami-ff0fea8310f3 --count 1 --instance-type t3.nano --key-name my-key --security-group-ids $sg_id --user-data file://./user_script_flask_api_example.sh
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
                    "InstanceId": "i-8008853b1b4473b29",
                    "InstanceType": "t3.nano",
                    "KernelId": "None",
                    "KeyName": "my-key",
                    "LaunchTime": "2024-04-01T08:00:36Z",
                    "Monitoring": {
                        "State": "disabled"
                    },
                    "Placement": {
                        "AvailabilityZone": "us-east-1a",
                        "GroupName": "",
                        "Tenancy": "default"
                    },
                    "PrivateDnsName": "ip-10-178-41-121.ec2.internal",
                    "PrivateIpAddress": "10.178.41.121",
                    "PublicDnsName": "ec2-54-214-140-132.compute-1.amazonaws.com",
                    "PublicIpAddress": "54.214.140.132",
                    "State": {
                        "Code": 0,
                        "Name": "pending"
                    },
                    "StateTransitionReason": "",
                    "SubnetId": "subnet-d0e68d61",
                    "VpcId": "vpc-8407b336",
                    "Architecture": "x86_64",
                    "BlockDeviceMappings": [
                        {
                            "DeviceName": "/dev/sda1",
                            "Ebs": {
                                "AttachTime": "2024-04-01T08:00:36Z",
                                "DeleteOnTermination": false,
                                "Status": "in-use",
                                "VolumeId": "vol-520eca06"
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
                                "PublicIp": "54.214.140.132"
                            },
                            "Attachment": {
                                "AttachTime": "2015-01-01T00:00:00Z",
                                "AttachmentId": "eni-attach-3e45f797",
                                "DeleteOnTermination": true,
                                "DeviceIndex": 0,
                                "Status": "attached"
                            },
                            "Description": "Primary network interface",
                            "Groups": [
                                {
                                    "GroupName": "default",
                                    "GroupId": "sg-6db4f403b96d01afd"
                                }
                            ],
                            "MacAddress": "1b:2b:3c:4d:5e:6f",
                            "NetworkInterfaceId": "eni-f4c2dca6",
                            "OwnerId": "000000000000",
                            "PrivateIpAddress": "10.178.41.121",
                            "PrivateIpAddresses": [
                                {
                                    "Association": {
                                        "IpOwnerId": "000000000000",
                                        "PublicIp": "54.214.140.132"
                                    },
                                    "Primary": true,
                                    "PrivateIpAddress": "10.178.41.121"
                                }
                            ],
                            "SourceDestCheck": true,
                            "Status": "in-use",
                            "SubnetId": "subnet-d0e68d61",
                            "VpcId": "vpc-8407b336"
                        }
                    ],
                    "RootDeviceName": "/dev/sda1",
                    "RootDeviceType": "ebs",
                    "SecurityGroups": [
                        {
                            "GroupName": "default",
                            "GroupId": "sg-6db4f403b96d01afd"
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
            "ReservationId": "r-3bdcfb12"
        }
</pre>

&nbsp;

&nbsp;

---

&nbsp;

You can now open the LocalStack logs to find the IP address of the locally emulated EC2 instance. Run the following command to open the LocalStack logs:
<pre>
    ❯ docker logs localstack
</pre>
You should see the following output:
<pre>
    2023-08-16T17:18:29.702  INFO --- [   asgi_gw_0] l.s.ec2.vmmanager.docker   : Instance i-b07acefd77a3c415f will be accessible via SSH at: 127.0.0.1:12862, 172.17.0.4:22
    2023-08-16T17:18:29.702  INFO --- [   asgi_gw_0] l.s.ec2.vmmanager.docker   : Instance i-b07acefd77a3c415f port mappings (container -> host): {'8000/tcp': 29043, '22/tcp': 12862}
</pre>


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
