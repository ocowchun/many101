##指定`sidekiq` 的redis的url
可以設定環境變數`REDIS_URL`來指定

```
REDIS_URL: redis://xxx.amazonaws.com:6379
```

####reference
https://github.com/mperham/sidekiq/wiki/Using-Redis

##cap task

Run cap -T sidekiq in the terminal to get a full list of the sidekiq commands:

```
cap sidekiq:quiet                  # Quiet sidekiq (stop processing new tasks)
cap sidekiq:respawn                # Respawn missing sidekiq proccesses
cap sidekiq:restart                # Restart sidekiq
cap sidekiq:rolling_restart        # Rolling-restart sidekiq
cap sidekiq:start                  # Start sidekiq
cap sidekiq:stop                   # Stop sidekiq
```

####reference
https://github.com/seuros/capistrano-sidekiq/wiki