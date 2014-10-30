#Monit
>誰都無法避免異常的出現，有些異常會導致處理過程出錯。異常的禍首可能是資料庫、Nginx、應用程序伺服器或後台作業程式。
我們希望系統盡量照顧好自己，如果檢測到異常，能自動重啟。如果系統無法處理，就要立即發出警報，讓我們手動修復。
Monit就具有這種功能，它會監控系統的各種參數，例如進程或網路接口。如果進程崩潰或者超出了預設的參數，Monit會重啟該進程。
如果無法重啟，或著某段時間重啟次數太多，Monit會通過郵件給我們發送警報。


####手動安裝Monit
```sh
sudo apt-get install monit
```

####啟動網頁介面
```
set httpd port 3737
  allow admin:monit
```
使用3737作為Monit網頁的port,登入時需要輸入帳號密碼

####追蹤Nginx
```
check process nginx with pidfile /var/run/nginx.pid
  start program = "/etc/init.d/nginx start"
  stop program = "/etc/init.d/nginx stop"
  if failed host 127.0.0.1 port 80 then restart
  if 15 restarts within 15 cycles then timeout
```

[Monit。我所使用的系統及服務監控軟體](http://portable.easylife.tw/2407)
