## Basic Setup

* Steps 1: <br/>
  Download Terraform from <https://www.terraform.io/downloads.html>
* Step 2:<br/>Extract it, set path
* Step 3 :<br/>

  ```cmd
  terraform init
  ```

* Step 4:<br/>
  
  * Basic AWS Spinning instance:   **demo.tf**

  ```json
    provider "aws" {
      access_key = "*********"
      secret_key = "**********"
      region  = "us-east-2"
    }

    resource "aws_instance" "basic_ec2" {
      ami           = "ami-02f706d959cedf892"
      instance_type = "t2.micro"
    }

  ```

* Step 5:

  ```cmd
  terraform apply
  ```

#### Generate SSH Keys

```cmd
Command To generate SSH key Pair
ssh-keygen -t rsa -C "your_email@example.com"
```


## Advanced File Structure


This is how the ***instance.tf***
```json
resource "aws_instance" "basic_ec2" {
    ami           = "${lookup(var.AMIS, var.AWS_REGION)}"
    instance_type = "t2.micro"
}
```
This is how the ***provider.tf***
```json
provider "aws" {
    access_key = "${var.AWS_ACCESS_KEY}"
    secret_key = "${var.AWS_SECRET_KEY}"
    region = "${var.AWS_REGION}"
}

This is how the  ***vars.tf***
```json
variable "AWS_ACCESS_KEY" {}
variable "AWS_SECRET_KEY" {}
variable "AWS_REGION" {
    default = "us-east-2"
}

variable "AMIS" {
  type = "map"
  default = {
    us-east-2 = "ami-02f706d959cedf892"
    us-west-2 = "ami-06b94666"
    eu-west-1 = "ami-0d729a60"
  }
}
```

This is how the ***terraform.tfvars***

```json
AWS_ACCESS_KEY = "*******"
AWS_SECRET_KEY = "********"
AWS_REGION = "us-east-2"
```


## Some Concepts

Q] What are Provisioner?

Provisioners are used to execute scripts on a local or remote machine as part of resource creation or destruction.
Provisioners can be used to bootstrap a resource, cleanup before destroy, run configuration management, etc.

```json
resource "aws_key_pair" "my_key" {                // First we need to send AWS keypair to Amazon
  key_name = "myKey"
  public_key = "${file("${var.PATH_TO_PUBLIC_KEY}")}"
}

resource "aws_instance" "basic_ec2" {
    ami           = "${lookup(var.AMIS, var.AWS_REGION)}"
    instance_type = "t2.micro"
    key_name = "${aws_key_pair.my_key.key_name}"
    provisioner "file" {

    }

    provisioner "file" {                   //This is a file provisioner which will uplaod script.sh in the destination path
    source = "script.sh"
    destination = "/tmp/script.sh"
    }
    provisioner "remote-exec" {          //remote-execute will change the access and execute the script
    inline = [
      "chmod +x /tmp/script.sh",
      "sudo /tmp/script.sh"
        ]
    }
   
    connection {
        host = "${self.public_ip}"
        user = "${var.INSTANCE_USERNAME}"
        private_key = "${file("${var.PATH_TO_PRIVATE_KEY}")}"
    }
}
```

***vars.tf***
```json
variable "AWS_ACCESS_KEY" {}
variable "AWS_SECRET_KEY" {}
variable "AWS_REGION" {
    default = "us-east-2"
}

variable "AMIS" {
  type = "map"
  default = {
    us-east-2 = "ami-02f706d959cedf892"
    us-west-2 = "ami-06b94666"
    eu-west-1 = "ami-0d729a60"
  }
}

variable "PATH_TO_PRIVATE_KEY" {
  default = "mykey"
}

variable "PATH_TO_PUBLIC_KEY" {
  default = "mykey.pub"
}

variable "INSTANCE_USERNAME" {
  default = "ec2-user"
}
```
