# How to push a folder from Java Code to s3 by Command Line

## AWS CLI Installation

#### [In Windows AWS CLI Installation](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html#cliv2-linux-install)
* Download the AWS CLI MSI installer for Windows (64-bit) at https://awscli.amazonaws.com/AWSCLIV2.msi
* Install it

#### [In EC2 AWS CLI Installation](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html#cliv2-linux-install)
* curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
* unzip awscliv2.zip
* sudo ./aws/install

## AWS Version Check
* aws --version

## [AWS Configure](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)
* ***aws configure***<br/>
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE<br/>
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY<br/>
Default region name [None]: us-west-2<br/>
Default output format [None]: json<br/>
