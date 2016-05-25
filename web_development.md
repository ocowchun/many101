#web 開發注意事項
##討論時的注意事項
* 空狀態,錯誤狀態的處理
* 桌面版,手機版
* 圖片的樣式要加註解(i.e. 使用者的圖片如果背景是透明的時候,是否要加入背景色)

##開發前
* **使用[figaro](https://github.com/laserlemon/figaro)，管理所有token!!!!!**
* 準備production環境
* 使用[sprockets better error](sprockets_better_errors)，錯誤測試
* 使用webpack來管理所有前端工具，盡量避免使用bower。

##開發中
* **不要使用remote 回傳js的寫法，IE比較舊的版本不支援!!!**
* 靜態檔案需要加入precompile清單
* model relation建立，需包含dependent
* form 一律使用 form_for 及 form_tag，並用partial。
* partial裡面不放js
* helper（使用get, set，通用寫在application，html多就寫在partial）
* 複雜的view使用[cells](https://github.com/apotonick/cells)
* 使用Restful的view，至多增加layout, common
* 建立`doc/schema.md` db有變動時，就把內容寫進去

###協助開發的gem

####pry-rails
>設定中斷點

[link](https://github.com/rweng/pry-rails)
[Pry ：新一代 Debug 利器](http://blog.xdite.net/posts/2012/08/13/pry-the-new-debugger)

####better_errors
>Better Errors replaces the standard Rails error page with a much better and more useful error page. It is also usable outside of Rails in any Rack app as Rack middleware.

[link](https://github.com/charliesome/better_errors)

####quiet_assets
>Mutes assets pipeline log messages.

[quiet_assets](https://github.com/evrone/quiet_assets)


####rails 4.2以前的版本，使用[foreigner](https://github.com/matthuhiggins/foreigner)建立fk
格式如下:
```rb
    add_foreign_key "child_table", "parent_table", dependent: :delete, name: "child_xxx_id_fk"
```

才會確保說刪除資料時,會把關聯的資料一併刪除


##發佈前必須完成的事項

###一定要做的事情!!!!
* 安裝並設定好[rollbar](https://rollbar.com/)
* load test

####avoid error for apple-touch-icon-precomposed.png
To resolve, add 2 100×100 png files, save it as apple-touch-icon-precomposed.png and apple-touch-icon.png and upload it to the root directory of the server. After that, the error should be gone.

####robots.txt
要先設定好，哪些網址不要讓機器人看(例如ajax,api)

##發布後
在部署完成的網站,使用chrome developer console,確認所有靜態資源都有讀取到

###gem
* annotate
* simple_form
* i18n
* figaro
