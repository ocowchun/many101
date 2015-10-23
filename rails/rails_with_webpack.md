#如何整合Rails與webpack

我們將前端的 JavaScript 都放在專案根目錄下面的 `js` 資料夾裡面，然後透過 webpack 將檔案輸出到 `app/assets/javascripts/bundle`這個資料夾讓 Rails 可以讀取

##基本版
>只有一個共用的app.js

###1. 在專案的根目錄設定 `webpack.config.js`

```js
var path = require('path');
var webpack = require('webpack');

module.exports = {
  devtool: 'eval',
  entry: [
    './js/entry/app'
  ],
  output: {
    // path: path.join(__dirname, 'dist'),
    path: path.join(__dirname, 'app', 'assets', 'javascripts', 'bundle'),
    filename: 'bundle.js',
    publicPath: '/static/'
  },
  plugins: [
    new webpack.HotModuleReplacementPlugin(),
    new webpack.NoErrorsPlugin()
  ],
  module: {
    loaders: [{
      test: /\.js$/,
      loaders: ['babel'],
      include: path.join(__dirname, 'js')
    }]
  }
};
```

###2. 安裝相關的 node packages

```bash
$ npm init #如果你的專案根目錄沒有package.json
$ npm install webpack babel-core --save-dev
```

###3. 在`config/application.rb`設定 precompile assets
```rb
Rails.application.config.assets.precompile += %w( bundle/bundle.js )
```

###4. 在開發時執行
```bash
$ webpack -w
```

##多入口版
>因應不同的頁面需求，會有對應的 JavaScript

###1. 在專案的根目錄設定 `webpack.config.js`

```js
var path = require('path');
var webpack = require('webpack');

module.exports = {
  devtool: 'eval',
  entry: {
    page1:'./js/entry/page1',
    page2:'./js/entry/page2'
  },
  output: {
    // path: path.join(__dirname, 'dist'),
    path: path.join(__dirname, 'app', 'assets', 'javascripts', 'bundle'),
    filename: '[name]-bundle.js',
    publicPath: '/static/'
  },
  plugins: [
    new webpack.HotModuleReplacementPlugin(),
    new webpack.NoErrorsPlugin()
  ],
  module: {
    loaders: [{
      test: /\.js$/,
      loaders: ['babel'],
      include: path.join(__dirname, 'js')
    }]
  }
};
```

###2. 安裝相關的 node packages

```bash
$ npm init #如果你的專案根目錄沒有package.json
$ npm install webpack babel-core --save-dev
```

###3. 在`config/application.rb`設定 precompile assets
```rb
Rails.application.config.assets.precompile += %w( bundle/*.js)
```

###4. 在開發時執行
```bash
$ webpack -w
```

##hot module reload & react 版本

###1. 在專案的根目錄設定 `webpack.config.js`

```js
var path = require('path');
var webpack = require('webpack');

module.exports = {
  devtool: 'eval',
  entry: {
    page1:'./js/entry/page1',
    page2:'./js/entry/page2'
  },
  output: {
    // path: path.join(__dirname, 'dist'),
    path: path.join(__dirname, 'app', 'assets', 'javascripts', 'bundle'),
    filename: '[name]-bundle.js',
    publicPath: '/static/'
  },
  plugins: [
    new webpack.HotModuleReplacementPlugin(),
    new webpack.NoErrorsPlugin()
  ],
  module: {
    loaders: [{
      test: /\.js$/,
      loaders: ['babel'],
      include: path.join(__dirname, 'js')
    }]
  }
};
```

###2. 設定 `devServer.js` 檔案

```js
var path = require('path');
var express = require('express');
var webpack = require('webpack');
var rawConfig = require('./webpack.config');
var _ = require('underscore');

var oldEntry = rawConfig.entry;
var entries = _.keys(oldEntry);
var newEntry = {};

var hmrClient = 'webpack-hot-middleware/client?path=http://localhost:5000/__webpack_hmr&reload=true';

_.each(entries, function(entry) {
  newEntry[entry] = _.flatten([hmrClient, oldEntry[entry]]);
});

var config=_.extend(rawConfig,{entry:newEntry});

var app = express();
var compiler = webpack(config);

app.use(require('webpack-dev-middleware')(compiler, {
  noInfo: true,
  publicPath: config.output.publicPath
}));

app.use(require('webpack-hot-middleware')(compiler));

app.get('*', function(req, res) {
  res.sendFile(path.join(__dirname, 'index.html'));
});

app.listen(5000, 'localhost', function(err) {
  if (err) {
    console.log(err);
    return;
  }

  console.log('Listening at http://localhost:5000');
});
```

###3. 設定 `.babelrc`
```js
{
  "stage": 0,
  "env": {
    "development": {
      "plugins": ["react-transform"],
      "extra": {
        "react-transform": {
          "transforms": [{
            "transform": "react-transform-hmr",
            "imports": ["react"],
            "locals": ["module"]
          }, {
            "transform": "react-transform-catch-errors",
            "imports": ["react", "redbox-react"]
          }]
        }
      }
    }
  }
}
```

###4. 安裝相關的 node packages

```bash
$ npm init #如果你的專案根目錄沒有package.json
$ npm install webpack babel-core babel-plugin-react-transform express react-transform-catch-errors react-transform-hmr webpack-dev-middleware webpack-hot-middleware --save-dev
```

###5. 在`config/application.rb`設定 precompile assets
```rb
Rails.application.config.assets.precompile += %w( bundle/*.js)
```

###6. 在lib資料夾加入 `hmr_proxy.rb`
```rb
require 'rack-proxy'
class HmrProxy < Rack::Proxy
  def initialize(app)
    @streaming = false
    @ssl_verify_none =  false
    @read_timeout =  1
    @app = app
  end

  def call(env)
    original_host = env["HTTP_HOST"]
    rewrite_env(env)
    if env["HTTP_HOST"] != original_host
      perform_request(env)
    else
      # just regular
      @app.call(env)
    end
  end

  def rewrite_env(env)
    if env["PATH_INFO"].start_with?('/static/')
      env["HTTP_HOST"] = "localhost:5000"
    end
    env
  end

end
```

###7. 在 `config/environments/development.rb` 設定 `HmrProxy`

```rb
require_relative '../../lib/hmr_proxy'

Rails.application.configure do
  config.middleware.use HmrProxy
...
end
```

###. 在開發時執行
```bash
$ node devServer
```

[hmr-rails-boilerplate](https://github.com/ocowchun/hmr-rails-boilerplate)


##是否要將app/assets/javascripts/bundle/*.js 納入 git?
因為專案會同時有多人編輯 JavaScript ，如果將 `app/assets/javascripts/bundle/*.js` 納入 git 會有解不完的衝突。
目前我們的做法是將 `/app/assets/javascripts/bundle/` 加到 `.gitignore` ，然後再執行 `rake assets:precompile` 前先執行 `webpack`


