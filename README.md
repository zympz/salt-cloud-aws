# Working with Salt-Cloud on AWS EC2


## Installation

### Salt-Cloud

As of November 8th, 2013 `salt-cloud` is part of the main `salt` repository and
does not need to be installed separately from `salt-master`. The legacy
`salt-cloud` package is still available in most community driven repositories
like `EPEL` and `Universe`.

    apt-get -y install salt-master

## Configuration

The configuration files we will need to create are the cloud config at
`/etc/salt/cloud` and cloud profiles at `/etc/salt/cloud.profiles`.

### Salt Cloud Configuration File

The [cloud](salt/cloud) file should be placed at `/etc/salt/cloud`

### Salt Cloud Profiles Configuration File

The [cloud.profiles](salt/cloud.profiles) file should be placed at
`/etc/salt/cloud.profiles`

### AWS User Configuration

Log into AWS and go to the [Identity and Access Management](https://console.aws.amazon.com/iam/home#home)
(IAM) page.

Create a user to be used by `salt-cloud` under the __Users__ tab.

Create an __Access Key__ for your user, it will send you an excel file with the
`id` and `key`, paste these into `/etc/salt/cloud`. You will not be able to
download this excel file which contains the `key` again, if you lose the `key`
you will have to generate a new one.

Go to the [EC2 Dashboard](https://console.aws.amazon.com/ec2/v2/home) and select
__Key Pairs__ under __Network & Security__, create a key pair. You will download
the `.pem` file and store it on the `salt-master`. Again, you will not be able
to retrieve this private key a second time, if you lose it you will have to
create a new one.

    mkdir /etc/salt/aws
    /etc/salt/aws/us-east.pem
    chmod 0400 /etc/salt/aws/us-east.pem

You will need a __Key Pair__ for every region in __EC2__ you want to deploy
instances via salt to, which will directly translate to a new provider section
in `/etc/salt/cloud`.
 
* us-east-1 (N. Virginia)
* us-west-1 (N. California)
* us-west-2 (Oregon)
* eu-west-1 (Ireland)
* ap-southeast-1 (Singapore)
* ap-northeast-1 (Tokyo)
* ap-southeast-2 (Sydney)
* sa-east-1 (Sao Paulo)

After saving your private key to the `salt-master`, update the values for
`private_key` and `keyname` under the appropriate
provider section in `/etc/salt/cloud`.

* `private_key` - full path on OS to private key file
* `keyname` - name you provided when creating the key
