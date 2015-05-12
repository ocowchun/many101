###如何讓mocha支援es6(使用babel)
 1. 安裝`babel`
 ```bash
 $ npm install --save-dev babel
 ```

 2.修改`package.json`

 ```js
  "scripts": {
    "test": "mocha --compilers js:babel/register"
  },
```
