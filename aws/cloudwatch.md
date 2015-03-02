- Configuration file successfully saved at: /var/awslogs/etc/awslogs.conf
- You can begin accessing new log events after a few moments at https://console.aws.amazon.com/cloudwatch/home?region=ap-northeast-1#logs:
- You can use 'sudo service awslogs start|stop|status|restart' to control the daemon.
- To see diagnostic information for the CloudWatch Logs Agent, see /var/log/awslogs.log
- You can rerun interactive setup using 'sudo python ./awslogs-agent-setup.py --region ap-northeast-1 --only-generate-config'
[how to start](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/QuickStartEC2Instance.html)


```sh
$ sudo apt-get update
$ wget https://s3.amazonaws.com/aws-cloudwatch/downloads/latest/awslogs-agent-setup.py
$ sudo python ./awslogs-agent-setup.py --region ap-northeast-1
```


##how to monitor ec2 memory

```sh
sudo apt-get install unzip libwww-perl libcrypt-ssleay-perl
sudo apt-get install libswitch-perl
wget http://ec2-downloads.s3.amazonaws.com/cloudwatch-samples/CloudWatchMonitoringScripts-v1.1.0.zip
unzip CloudWatchMonitoringScripts-v1.1.0.zip
rm CloudWatchMonitoringScripts-v1.1.0.zip
cd aws-scripts-mon
crontab -e 
```

輸入
```
*/5 * * * * ~/aws-scripts-mon/mon-put-instance-data.pl --mem-util --mem-used --mem-avail --from-cron
```

[Amazon CloudWatch Monitoring Scripts for Linux](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/mon-scripts-perl.html)