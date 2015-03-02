####Installing the Cordova CLI
```sh
 $ sudo npm install -g cordova
```
####Create the App
```sh
$ cordova create hello com.example.hello HelloWorld
```

####Add Platforms
```sh
$ cordova platform add ios
```

>WARNING: When using the CLI to build your application, you should not edit any files in the /platforms/ directory unless you know what you are doing, or if documentation specifies otherwise. The files in this directory are routinely overwritten when preparing applications for building, or when plugins are reinstalled.

[reference](http://cordova.apache.org/docs/en/edge/guide_cli_index.md.html#The%20Command-line%20Interface)