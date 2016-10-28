#Packer
>自動打包image

##install packer on mac
	$ brew tap homebrew/binary
	$ brew install packer

###Alternative Installation Methods
[set up](http://www.packer.io/intro/getting-started/setup.html)

##Build an Image

### 1.create `example.json`

```json
{
  "variables": {
    "aws_access_key": "{{env `AWS_ACCESS_KEY_ID`}}",
    "aws_secret_key": "{{env `AWS_SECRET_ACCESS_KEY`}}"
  },
  "builders": [{
    "type": "amazon-ebs",
    "access_key": "{{user `aws_access_key`}}",
    "secret_key": "{{user `aws_secret_key`}}",
    "region": "ap-northeast-1",
    "vpc_id": "vpc-12345678",
    "source_ami": "ami-a21529cc",
    "instance_type": "t2.micro",
    "ssh_username": "ubuntu",
    "ami_name": "packer-example {{timestamp}}"
  }]
}
```

### 2. excute `packer build example.json`

##PROVISION
> install and configure software into the images as well


http://engineering.cotap.com/post/78783269747/hello-world-using-packer-chef-and-berkshelf-on


berks vendor vendor/cookbooks


[DEBUGGING PACKER BUILDS](https://www.packer.io/docs/other/debugging.html)
```bash
$ packer build -debug
```


## packer with cookbook
