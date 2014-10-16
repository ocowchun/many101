#web 開發注意事項
##開發前
* 準備production環境
* 使用sprockets better error，錯誤測試
* 使用application.yml (figaro)，管理所有token
* 使用bower，管理所有前端工具

##開發中
* **不要使用remote 回傳js的寫法，IE比較舊的版本不支援!!!**
* 靜態檔案需要加入precompile清單
* model relation建立，需包含dependent
* form 一律使用 form_for 及 form_tag，並用partial。
* partial裡面不放js
* helper（使用get, set，通用寫在application，html多就寫在partial）
* 複雜的view使用cell
* 使用Restful的view，至多增加layout, common

##發佈
* rollbar
* load test
####avoid error for apple-touch-icon-precomposed.png
To resolve, add 2 100×100 png files, save it as apple-touch-icon-precomposed.png and apple-touch-icon.png and upload it to the root directory of the server. After that, the error should be gone.

####robots.txt
要先設定好，哪些網址不要讓機器人看(例如ajax,api)

###gem
* annotate
* simple_form
* i18n
* figaro
