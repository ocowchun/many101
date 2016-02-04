Use images that are at least 1200 x 630 pixels
https://developers.facebook.com/docs/sharing/best-practices#images


```js
function sendShareFeed(options, series) {
  var options = options || {};
  var lang = options.lang;

  var feedOptions = {
    method: 'feed',
    link: 'https://google.com',
    picture: 'https://abc.tw/hello.png',
    caption: 'hello.com.tw',
    description: 'foobar',
    name: 'guagua'
  };
  FB.ui(feedOptions, function(response) {
    afterShare(response, lang, series);
  });
}
```

