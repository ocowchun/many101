


在指定repo設定webhooks
example
`https://cwyfjhgjbg.localtunnel.me/events`

`$ curl https://api.github.com/?access_token=your_personal_access_tokens`

{
  "ref": "master",
  "payload": "{"user":"ocowchun"}",
  "description": "Deploying my sweet branch"
}


events#create 會建立一個`Receiver`的job

如果一切符合情況(repo正常,狀態是active...)的話，會建立一個`Heaven::Jobs::Deployment`的job
Heaven::Provider用工廠模式建立適合的Provider(i.e. heroku,cap...etc)
Provider負責deployment