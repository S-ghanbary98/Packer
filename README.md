# Packer

![alt text](img/icon.png)

##### What is Packer?
- Packer is an open source tool for creating identical machine images for multiple platforms from a single source configuration. 

### Packer Installation

```python
 curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install packer
```

### Configuration For Building AMI 

- Create and enter a folder called packer via `sudo mkdir packer`.
- In the directory create a `aws-ubuntu.pkr.hcl`. That contains the following.

```python
packer {
  required_plugins {
    amazon = {
      version = ">= 0.0.2"
      source  = "github.com/hashicorp/amazon"
    }
  }
}

source "amazon-ebs" "ubuntu" {
  ami_name      = "eng89_packer_shervin"
  instance_type = "t2.micro"
  region        = "eu-west-1"
  source_ami_filter {
    filters = {
      name                = "ubuntu/images/*ubuntu-xenial-16.04-amd64-server-*"
      root-device-type    = "ebs"
      virtualization-type = "hvm"
    }
    most_recent = true
    owners      = ["099720109477"]
  }
  ssh_username = "ubuntu"
}

build {
  name = "learn-packer"
  sources = [
    "source.amazon-ebs.ubuntu"
  ]
}
```
- We now have to initialize our access and secret access keys via the following
```python
$ export AWS_ACCESS_KEY_ID=YOUR_ACCESS_KEY
$ export AWS_SECRET_ACCESS_KEY=YOUR_SECRET_KEY
```

### Initialisation and Build

- Initialize your Packer configuration. `sudo packer init .`
- Format and validate template. `sudo  packer fmt .` && `sudo packer validate .`
- Build AMI. `packer build aws-ubuntu.pkr.hcl`

![alt text](img/Capture.PNG)
