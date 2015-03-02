##gulp practice


[getting started](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md)

要先安裝`node`!!!

###1.先幫系統安裝`gulp`

```
npm install -g gulp
```

###2.幫你的project安裝`gulp`

```
npm install gulp --save-dev
```

###3.建立`gulpfile.js`在專案的根目錄

``` javascript
var gulp=require('gulp');
gulp.task('default',function(){
});
```

###4.執行gulp

在專案目錄下的commandline輸入
```
$ gulp
```
會執行`default`task，不過因為`default`裡面什麼也沒有，所以不會做任何事。

###5.幫你的project安裝`browser-sync`
```
npm install browser-sync --save-dev
```

###6.修改你的`gulpfile.js`加入`browser-sync`
```javascript
var gulp = require('gulp');
var browserSync = require('browser-sync');

gulp.task('browser-sync', function() {
	browserSync({
		server: {
			baseDir: "app",
			routes: {
				"/bower_components": "bower_components"
			}
		}
	});
});


// Reload all Browsers
gulp.task('bs-reload', function() {
	browserSync.reload();
});


gulp.task('default', ['browser-sync'], function() {
	gulp.watch("app/**/*.*", ["bs-reload"]);
	// body...
});
```


###7.再執行一次`gulp`
```
$ gulp
```
###reference
[browser-sync api](http://www.browsersync.io/docs/api/)
