[hubot-heroku-keepalive](https://github.com/hubot-scripts/hubot-heroku-keepalive)
##設定下列三個變數來確保他在指定時間內會清醒
* `HUBOT_HEROKU_KEEPALIVE_URL` 
 required, the URL to keepalive

* `HUBOT_HEROKU_WAKEUP_TIME` 
optional, the time of day (HH:MM) when hubot should wake up. Default: 6:00 (6 am)

* `HUBOT_HEROKU_SLEEP_TIME` 
optional, the time of day (HH:MM) when hubot should go to sleep. Default: 22:00 (10 pm)

##設定heroku scheduler來叫醒heroku app
```sh
$ heroku addons:create scheduler:standard
$ heroku addons:open scheduler
```

The scheduler must be manually configured from the web interface, so run heroku addons:open scheduler and configure it to run curl ${HUBOT_HEROKU_KEEPALIVE_URL}heroku/keepalive at the time configured for HUBOT_HEROKU_WAKEUP_TIME.

https://github.com/hubot-scripts/hubot-heroku-keepalive#waking-hubot-up

##[hubot-cron](https://github.com/miyagawa/hubot-cron)
miyagawa> hubot new job 0 9 * * 1-5 "Good morning everyone!"
hubot> Job 12345 created

miyagawa> hubot list jobs
hubot> (list of jobs)

miyagawa> hubot rm job 12345
hubot> Job 12345 removed

miyagawa> hubot tz job 12345 America/Los_Angeles
hubot> Job 12345 updated to use America/Los_Angeles