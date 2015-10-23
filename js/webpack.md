#webpack
>將使用 commonjs 的方式撰寫的 JavaScript 檔案打包起來

##優點:
* 讓我們可以輕鬆的模組化 JavaScript 檔案
* 讓前端的 JavaScript也可以使用npm的packages
* 方便開發react

##常用設定
http://webpack.github.io/docs/configuration.html

###[entry](https://webpack.github.io/docs/configuration.html#entry)
設定編譯的入口檔案

###[output](https://webpack.github.io/docs/configuration.html#output)
設定編譯完成的 JavaScript 檔案的命名方式與輸出的資料夾

###[externals](https://webpack.github.io/docs/configuration.html#externals)
將檔案的相依性使用全域變數來處理

###[module](https://webpack.github.io/docs/configuration.html#module)
對指定類型的檔案進行處理(i.e. 將寫 es6 的 JavaScript 檔案透過 babel 轉成es5 的 JavaScript 檔案)

###[plugins](https://webpack.github.io/docs/configuration.html#plugins)
額外的插件，通常是用來協助開發或用(i.e. HotModuleReload)