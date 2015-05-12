使用test kitchen

###安裝ec2 driver
chef gem install kitchen-ec2

###建立kitchen
```sh
mkdir kitchen101
cd kitchen101
kitchen init --driver=kitchen-ec2
```



###設定參數
####kitchen.yml
```yml 
---
driver:
  name: ec2
  aws_access_key_id: aws_access_key_id
  aws_secret_access_key: aws_access_key_id
  aws_ssh_key_id: pem_name
  ssh_key: pem_path
  security_group_ids: ["sg-98xxxx"]
  region: ap-northeast-1
  availability_zone: ap-northeast-1a
  require_chef_omnibus: true
  subnet_id: subnet-6d6...
  iam_profile_name: chef-client
  ssh_timeout: 10
  ssh_retries: 5
  ebs_volume_size: 6,
  ebs_delete_on_termination: 'true'
  ebs_device_name: '/dev/sda'  
  subnet_id: subnet-318xxxx
provisioner:
  name: chef_solo

platforms:
  - name: ubuntu-14.04
    driver:
      image_id: ami-df4b60de
      username: ubuntu

suites:
  - name: default
    run_list: ec101::default
    attributes:

```
