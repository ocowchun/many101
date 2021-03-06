#chef 101

##chef是做什麼用的?
把安裝package(i.e. 安裝mysql到ubuntu)這件事情自動化的工具

想想看平常安裝rails production環境的時候，要打一堆`apt-get install`，有了`chef`之後，這些事情可以寫成`cookbooks`，以後就能自動化完成，再也不用跟環境在哪邊過不去囉!!


####install chefDk
[downloads](http://downloads.getchef.com/chef-dk/mac/#/)

###quick start
####初始化kitchen project
```sh
chef exec kitchen init 
```
####新增metadata.rb
```rb
name "cookbook-name"
version "0.0.1"
maintainer "chef-name"
maintainer_email "chef@chef.com"
supports "ubuntu"
```

####新增Berksfile
```
source "https://api.berkshelf.com"

metadata
```

####新增`default.rb`到`recipes`資料夾
```rb
package 'git' #安裝git
```

####修改`.kitchen.yml`
```yml
suites:
  - name: default
    run_list: cookbook-name::default
    attributes:
```

####開始安裝機器!!
```sh
chef exec kitchen converge
```

這樣就完成了!

###常用指令
####help
```
chef exec kitchen help
```

####初始化kitchen
```
chef exec kitchen init 
```

####安裝機器
```
chef exec kitchen converge
```

####關掉機器(機器會被砍掉喔!!!)
```
chef exec kitchen destroy 
```

####Log in to one instance
```
chef exec kitchen login 
```


####[more](http://kitchen.ci/docs/getting-started/getting-help)



###test kitchen
an integration tool for developing and testing infrastructure code

```
kitchen init
```

####kitchen.yml

```yml

---
driver:
  name: vagrant

provisioner:
  name: chef_solo

platforms:
  - name: ubuntu-14.04
    driver:
      network:
      - ["forwarded_port", {guest: 80, host: 8080}]

suites:
  - name: default

```

driver是你開虛擬機的工具(ex:vagrant,ec2)

####platforms
你可以放不同的os

####suites
如果你有很多設置，email server ,worker,..etc你可以在這邊設置。

###vagrant
support provision tools not only for VirtualBox(you can with it with ec2....)

###Test Kitchen supported platforms
```
kitchen driver discover
```
azure,ec2,.....etc

####create instance
	kitchen create ubuntu

####login to instance
	kitchen login ubuntu

###cookbook
####cookbook
你可以把它當成一個你要的資源
####recipes

###Defind cookbook
name
version

##Resources
you can think of Chef resources as wrapper of... system resources. To name a few things built things
directory
package
bash
cron


####package
default action是install


###write test for your cookbook
immutable means not changeable,and there're nenefits

1. Reduce inconsitency
2. Improve the trust into your deployment process
3. The whole process is preatable,hence
	* is's easiey to recover,scale
	* it's testable

##Let's write our specs

##Berksfile
is Gemfile for Chef

###CREATING Berksfile
put the following codes into `Berksfile`

```
source "https://supermarket.getchef.com"
metadata
```

注意cookbook的來源，要指定清楚，不然會搞混


###packer
http://www.packer.io/	
客制化自己的image

###LWRP(Lightweight Resources and Providers)

```rb
windows_package "7-Zip 9.20 (x64 edition)" do
  source "http://downloads.sourceforge.net/sevenzip/7z920-x64.msi"
  action :install
end
```
[http://dougireton.com/blog/2012/12/31/creating-an-lwrp/](Creating an LWRP, Part 1: The Resource)

###Cookbook Pattern
[The Environment Cookbook Pattern](http://blog.vialstudios.com/the-environment-cookbook-pattern/#thewrappercookbook)

###reference
[Chef & Immutable Infrasturcture by Richard Lee](https://speakerdeck.com/dlackty/chef-and-immutable-infrasturcture)
