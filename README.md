<h1>AWS Simple EC2 CLI</h1>

<h4>A CLI tool that simplifies the process of launching, connecting and terminating an EC2 instance.</h4>

<p>
	<a href="https://golang.org/doc/go1.15">
	<img src="https://img.shields.io/github/go-mod/go-version/aws/amazon-ec2-metadata-mock?color=blueviolet" alt="go-version">
	</a>
	<a href="https://opensource.org/licenses/Apache-2.0">
	<img src="https://img.shields.io/badge/License-Apache%202.0-ff69b4.svg?color=orange" alt="license">
	</a>
	<a href="https://goreportcard.com/report/github.com/awslabs/aws-simple-ec2-cli">
	<img src="https://goreportcard.com/badge/github.com/awslabs/aws-simple-ec2-cli" alt="go-report-card">
	</a>
  <a href="https://travis-ci.com/awslabs/aws-simple-ec2-cli">
	<img src="https://travis-ci.com/awslabs/aws-simple-ec2-cli.svg?branch=main" alt="build-status">
  </a>
</p>


d

<div>
  <hr>
</div>

## Summary

In order to launch a new EC2 instance, customers need to specify a lot of options, and it can be a slow and overwhelming task. It requires users to have an initial network stack (VPC-Id/Subnet-Id/Security-Groups), remote login, and many more. Often times, we require EC2 instance for adhoc testing for a short period of time without requiring complex networking infrastructure in place. AWS Simple EC2 CLI aims to solve this issue and make it easier for users to launch, connect and terminate EC2 instances with a single command

## Major Features

- Launch an instance using single command
- Connect to an instance using single command
- Terminate an instance using single command
- Interactive mode that help users to decide parameters to use
- Config file for more convenient launch

## Installation and Configuration
1. Install AWS CLI

To execute the CLI, you will need AWS credentials configured. Take a look at the [AWS CLI configuration documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html#config-settings-and-precedence) for details on the various ways to configure credentials. Alternatively, you can try out the AWS Simple EC2 CLI by populating following environment variables:

```
export AWS_ACCESS_KEY_ID="..."
export AWS_SECRET_ACCESS_KEY="..."
# Set default Region (optional)
export AWS_REGION="us-east-1" 
```

2. Install AWS Simple EC2 CLI w/ Curl
#### MacOS/Linux
```
curl -Lo simple-ec2 https://github.com/awslabs/aws-simple-ec2-cli/releases/download/v0.6.0/simple-ec2-`uname | tr '[:upper:]' '[:lower:]'`-amd64
chmod +x simple-ec2
```

#### ARM Linux
```
curl -Lo simple-ec2 https://github.com/awslabs/aws-simple-ec2-cli/releases/download/v0.6.0/simple-ec2-linux-arm
```

```
curl -Lo simple-ec2 https://github.com/awslabs/aws-simple-ec2-cli/releases/download/v0.6.0/simple-ec2-linux-arm64
```

#### Windows
```
curl -Lo simple-ec2 https://github.com/awslabs/aws-simple-ec2-cli/releases/download/v0.6.0/simple-ec2-windows-amd64.exe
```

## Examples

### Launch

**All CLI Options**

```
$ simple-ec2 launch -h
Launch an Amazon EC2 instance with the default configurations. 
	All configurations can be overridden by configurations provided by configuration files or user input

Usage:
  simple-ec2 launch [flags]

Flags:
  -a, --auto-termination-timer int       The auto-termination timer for the instance in minutes
  -h, --help                             help for launch
  -m, --image-id string                  The image id of the AMI used to launch the instance
  -t, --instance-type string             The instance type of the instance
  -i, --interactive                      Interactive mode
  -k, --keep-ebs                         Keep EBS volumes after instance termination
  -l, --launch-template-id string        The launch template id with which the instance will be launched
  -v, --launch-template-version string   The launch template version with which the instance will be launched
  -r, --region string                    The region where the instance will be launched
  -c, --save-config                      Save config as a JSON config file
  -g, --security-group-ids strings       The security groups with which the instance will be launched
  -s, --subnet-id string                 The subnet id in which the instance will be launched
```

**Single Command Launch**

```
$ simple-ec2 launch

Please confirm if you would like to launch instance with following options: 
+----------------+-------------------------------------------------+
| Region         | us-east-2                                       |
| VPC            | My Default VPC(vpc-123example)                  |
| Subnet         | subnet-123example                               |
| Instance Type  | t2.micro                                        |
| Image          | ami-123example                                  |
| Security Group | Default SG(sg-123example)                       |
|                | simple-ec2 SSH Security Group(sg-123example)    |
| EBS Volumes    | /dev/xvda(gp2): 8 GiB                           |
+----------------+-------------------------------------------------+
[ yes / no ]
yes
Options confirmed! Launching instance...
Launch Instance Success!
Instance ID: i-123example
```

**Single Command Launch With Flags**

```
$ simple-ec2 launch -r us-east-2 -m ami-123example -t t2.micro -s subnet-123example -g sg-123example

Please confirm if you would like to launch instance with following options: 
+----------------+-------------------------------------------------+
| Region         | us-east-2                                       |
| VPC            | My Default VPC(vpc-123example)                  |
| Subnet         | subnet-123example                               |
| Instance Type  | t2.micro                                        |
| Image          | ami-123example                                  |
| Security Group | simple-ec2 SSH Security(sg-123example)          |
| EBS Volumes    | /dev/xvda(gp2): 8 GiB                           |
+----------------+-------------------------------------------------+
[ yes / no ]
yes
Options confirmed! Launching instance...
Launch Instance Success!
Instance ID: i-123example
```

**Interactive Mode Launch**

```
$ simple-ec2 launch -i

Select the region you wish to launch the instance: [default: us-east-2]
+------------------+---------------------------+------------------+-------------------------+
|      REGION      |        DESCRIPTION        |      REGION      |       DESCRIPTION       |
+------------------+---------------------------+------------------+-------------------------+
| 1.ap-northeast-1 | Asia Pacific (Tokyo)      | 2.ap-northeast-2 | Asia Pacific (Seoul)    |
| 3.ap-northeast-3 | Asia Pacific (Osaka)      | 4.ap-south-1     | Asia Pacific (Mumbai)   |
| 5.ap-southeast-1 | Asia Pacific (Singapore)  | 6.ap-southeast-2 | Asia Pacific (Sydney)   |
| 7.ca-central-1   | Canada (Central)          | 8.eu-central-1   | Europe (Frankfurt)      |
| 9.eu-north-1     | Europe (Stockholm)        | 10.eu-west-1     | Europe (Ireland)        |
| 11.eu-west-2     | Europe (London)           | 12.eu-west-3     | Europe (Paris)          |
| 13.sa-east-1     | South America (Sao Paulo) | 14.us-east-1     | US East (N. Virginia)   |
| 15.us-east-2     | US East (Ohio)            | 16.us-west-1     | US West (N. California) |
+------------------+---------------------------+------------------+-------------------------+
15

Select the launch template you wish to use: [default: Do not use launch template]
+--------+-----------------------------------+----------------+
| OPTION |          LAUNCH TEMPLATE          | LATEST VERSION |
+--------+-----------------------------------+----------------+
| 1.     | Test123(lt-123example)            | 1              |
| 2.     | MyTemplate(lt-123example)         | 3              |
| 3.     | SubnetsTest(lt-123example)        | 5              |
| 4.     | Do not use launch template        |
+--------+-----------------------------------+----------------+
4

Please confirm if you would like to launch instance with following options: [default: t2.micro]
1. I will enter the instance type
2. I need advice given vCPUs and memory
1

Please enter the instance type (eg. m5.xlarge, c5.xlarge): [default: t2.micro]
t2.nano

Loading images. This might take up to 1 minute. Please be patient. 

Please select or enter the AMI: [default: Latest Amazon Linux 2 image]
+--------+------------------+-----------------------+--------------------------+
| OPTION | OPERATING SYSTEM |       IMAGE ID        |      CREATION DATE       |
+--------+------------------+-----------------------+--------------------------+
| 1.     | Amazon Linux 2   | ami-123example        | 2020-07-24T20:40:27.000Z |
| 2.     | Ubuntu           | ami-123example        | 2020-07-16T20:05:04.000Z |
| 3.     | Amazon Linux     | ami-123example        | 2020-07-21T18:33:13.000Z |
| 4.     | Red Hat          | ami-123example        | 2020-04-23T17:22:24.000Z |
| 5.     | SUSE Linux       | ami-123example        | 2020-07-21T18:04:53.000Z |
| 6.     | Windows          | ami-123example        | 2020-07-15T08:51:57.000Z |
+--------+------------------+-----------------------+--------------------------+
[ any image id ]: Select the image id
3

What VPC would you like to launch into?[default: My Default VPC(vpc-123example)]
+--------+------------------------------------------------+---------------+
| OPTION |                      VPC                       |  CIDR BLOCK   |
+--------+------------------------------------------------+---------------+
| 1.     | My Default VPC(vpc-123example)                 | 172.31.0.0/16 |
| 2.     | New VPC(vpc-123example)                        | 172.31.0.0/16 |
| 3.     | Create new VPC with default CIDR and 3 subnets |
+--------+------------------------------------------------+---------------+
1

What subnet would you like to launch into?[default: My Default Subnet(subnet-123example)]
+--------+------------------------------------+-------------------+----------------+
| OPTION |               SUBNET               | AVAILABILITY ZONE |   CIDR BLOCK   |
+--------+------------------------------------+-------------------+----------------+
| 1.     | Default Subnet(subnet-123example)  | us-east-2c        | 172.31.32.0/20 |
| 2.     | subnet-123example                  | us-east-2a        | 172.31.0.0/20  |
| 3.     | subnet-123example                  | us-east-2b        | 172.31.16.0/20 |
+--------+------------------------------------+-------------------+----------------+
1

What security group would you like to use?
+--------+-------------------------------------------------+-------------------------------------------------------+
| OPTION |                 SECURITY GROUP                  |                      DESCRIPTION                      |
+--------+-------------------------------------------------+-------------------------------------------------------+
| 1.     | Default SSH SG(sg-123example)                   | launch-wizard-1 created 2020-06-07T19:45:39.448-04:00 |
| 2.     | simple-ec2 SSH Security Group(sg-123example)    | Created by simple-ec2 for SSH connection to instances |
| 3.     | Default SG(sg-123example)                       | default VPC security group                            |
| 4.     | Add all available security groups               |
| 5.     | Create a new security group that enables SSH    |
+--------+-------------------------------------------------+-------------------------------------------------------+
2

What security group would you like to use?
+--------+----------------------------------------------+-------------------------------------------------------+
| OPTION |                SECURITY GROUP                |                      DESCRIPTION                      |
+--------+----------------------------------------------+-------------------------------------------------------+
| 1.     | Default SSH SG(sg-123example)                | launch-wizard-1 created 2020-06-07T19:45:39.448-04:00 |
| 2.     | Default SG(sg-123example)                    | default VPC security group                            |
| 3.     | Add all available security groups            |
| 4.     | Create a new security group that enables SSH |
| 5.     | Don't add any more security group            |
+--------+----------------------------------------------+-------------------------------------------------------+
5

Please confirm if you would like to launch instance with following options: 
+------------------+-------------------------------------------------+
| Region           | us-east-2                                       |
| 1.VPC            | My Default VPC(vpc-123example)                  |
| 2.Subnet         | My Default Subnet(subnet-123example)            |
| 3.Instance Type  | t2.nano                                         |
| 4.Image          | ami-123example                                  |
| 5.Security Group | simple-ec2 SSH Security Group(sg-123example)    |
| EBS Volumes      | /dev/xvda(gp2): 8 GiB                           |
+------------------+-------------------------------------------------+
[ yes / no ]
yes
Options confirmed! Launching instance...
Launch Instance Success!
Instance ID: i-123example

Do you want to save the configuration above as a JSON file that can be used in non-interactive mode? [default: no]
[ yes / no ]
yes
Saving config...
Config successfully saved: /Users/$USER/.simple-ec2/simple-ec2.json
```

### Connect

**All CLI Options**

```
$ simple-ec2 connect -h
Connect to an Amazon EC2 Instance, given the region and instance id

Usage:
  simple-ec2 connect [flags]

Flags:
  -h, --help                 help for connect
  -n, --instance-id string   The instance id of the instance you want to connect to
  -i, --interactive          Interactive mode
  -r, --region string        The region in which the instance you want to connect locates

```

**Single Command Connect**

```
$ simple-ec2 connect -r us-east-2 -n i-123example
Last login: Wed Jul 29 21:01:45 2020 from 52.95.4.1

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
14 package(s) needed for security, out of 31 available
Run "sudo yum update" to apply all updates.
[ec2-user@ip-example ~]$ exit
logout

```

**Interactive Mode Connect**

```
$ simple-ec2 connect -i

Select the region you wish to launch the instance: [default: us-east-2]
+------------------+---------------------------+------------------+-------------------------+
|      REGION      |        DESCRIPTION        |      REGION      |       DESCRIPTION       |
+------------------+---------------------------+------------------+-------------------------+
| 1.ap-northeast-1 | Asia Pacific (Tokyo)      | 2.ap-northeast-2 | Asia Pacific (Seoul)    |
| 3.ap-northeast-3 | Asia Pacific (Osaka)      | 4.ap-south-1     | Asia Pacific (Mumbai)   |
| 5.ap-southeast-1 | Asia Pacific (Singapore)  | 6.ap-southeast-2 | Asia Pacific (Sydney)   |
| 7.ca-central-1   | Canada (Central)          | 8.eu-central-1   | Europe (Frankfurt)      |
| 9.eu-north-1     | Europe (Stockholm)        | 10.eu-west-1     | Europe (Ireland)        |
| 11.eu-west-2     | Europe (London)           | 12.eu-west-3     | Europe (Paris)          |
| 13.sa-east-1     | South America (Sao Paulo) | 14.us-east-1     | US East (N. Virginia)   |
| 15.us-east-2     | US East (Ohio)            | 16.us-west-1     | US West (N. California) |
+------------------+---------------------------+------------------+-------------------------+
15

Select the instance you want to connect to: 
+--------+---------------------+-------------+------------------------+
| OPTION |      INSTANCE       |   TAG-KEY   |       TAG-VALUE        |
+--------+---------------------+-------------+------------------------+
| 1.     | i-123example        | CreatedBy   | simple-ec2             |
|        |                     | CreatedTime | 2020-7-29 16:38:52 EDT |
| 2.     | i-123example        | CreatedBy   | simple-ec2             |
|        |                     | CreatedTime | 2020-7-29 16:35:48 EDT |
+--------+---------------------+-------------+------------------------+
2

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
14 package(s) needed for security, out of 31 available
Run "sudo yum update" to apply all updates.
[ec2-user@ip-example ~]$ exit
logout
```

### Terminate

**All CLI Options**

```
$ simple-ec2 terminate -h
Terminate Amazon EC2 Instances, given the region and instance ids

Usage:
  simple-ec2 terminate [flags]

Flags:
  -h, --help                   help for terminate
  -n, --instance-ids strings   The instance ids of the instances you want to terminate
  -i, --interactive            Interactive mode
  -r, --region string          The region in which the instances you want to terminate locates
```

**One Command Terminate**

```
$ simple-ec2 terminate -r us-east-2 -n i-123example
Terminating instances
Instances [i-123example] terminated successfully
```

**Interactive Terminate**

```
$ simple-ec2 terminate -i

Select the region you wish to launch the instance: [default: us-east-2]
+------------------+---------------------------+------------------+-------------------------+
|      REGION      |        DESCRIPTION        |      REGION      |       DESCRIPTION       |
+------------------+---------------------------+------------------+-------------------------+
| 1.ap-northeast-1 | Asia Pacific (Tokyo)      | 2.ap-northeast-2 | Asia Pacific (Seoul)    |
| 3.ap-northeast-3 | Asia Pacific (Osaka)      | 4.ap-south-1     | Asia Pacific (Mumbai)   |
| 5.ap-southeast-1 | Asia Pacific (Singapore)  | 6.ap-southeast-2 | Asia Pacific (Sydney)   |
| 7.ca-central-1   | Canada (Central)          | 8.eu-central-1   | Europe (Frankfurt)      |
| 9.eu-north-1     | Europe (Stockholm)        | 10.eu-west-1     | Europe (Ireland)        |
| 11.eu-west-2     | Europe (London)           | 12.eu-west-3     | Europe (Paris)          |
| 13.sa-east-1     | South America (Sao Paulo) | 14.us-east-1     | US East (N. Virginia)   |
| 15.us-east-2     | US East (Ohio)            | 16.us-west-1     | US West (N. California) |
+------------------+---------------------------+------------------+-------------------------+
15

Select the instance you want to terminate: 
+--------+---------------------+-------------+------------------------+
| OPTION |      INSTANCE       |   TAG-KEY   |       TAG-VALUE        |
+--------+---------------------+-------------+------------------------+
| 1.     | i-123example        | CreatedBy   | simple-ec2             |
|        |                     | CreatedTime | 2020-7-29 16:38:52 EDT |
| 2.     | i-456example        | CreatedBy   | simple-ec2             |
|        |                     | CreatedTime | 2020-7-29 16:35:48 EDT |
+--------+---------------------+-------------+------------------------+
1

Select the instance you want to terminate: 
+--------+--------------------------------+-------------+------------------------+
| OPTION |            INSTANCE            |   TAG-KEY   |       TAG-VALUE        |
+--------+--------------------------------+-------------+------------------------+
| 1.     | i-456example                   | CreatedBy   | simple-ec2             |
|        |                                | CreatedTime | 2020-7-29 16:35:48 EDT |
| 2.     | Don't add any more instance id |
+--------+--------------------------------+-------------+------------------------+
1
Terminating instances
Instances [i-123example i-456example] terminated successfully
```

## Building
For build instructions please consult [BUILD.md](./BUILD.md).

## Communication
If you've run into a bug or have a new feature request, please open an issue.

##  Contributing
Contributions are welcome! Please read our [guidelines](./CONTRIBUTING.md) and our [Code of Conduct](./CODE_OF_CONDUCT.md)

## License
This project is licensed under the [Apache-2.0](./LICENSE) License.
