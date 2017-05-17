## 使用WebStorm调试Vue

### 参考:

* [Debugging Webpack applications in WebStorm](https://blog.jetbrains.com/webstorm/2015/09/debugging-webpack-applications-in-webstorm/)
* [Debugging JavaScript in Chrome](https://www.jetbrains.com/help/webstorm/2017.1/debugging-javascript-in-chrome.html)
* [Configuring JavaScript Debugger and JetBrains Chrome Extension](https://www.jetbrains.com/help/webstorm/2017.1/configuring-javascript-debugger-and-jetbrains-chrome-extension.html)


### 注意事项:

0. Chrome需要安装JetBrains[插件](https://chrome.google.com/webstore/detail/jetbrains-ide-support/hmhgeddbohgjknpmjagkdomcpobmllji)

1. 使用WebStorm内置的server作为调试服务器时，因为它默认带了项目名作为URL前缀，会导致在调试时js和css文件找不到。 如[WebStorm built-in web server gets 404 for every css and js file included in index.html](http://stackoverflow.com/questions/32381302/webstorm-built-in-web-server-gets-404-for-every-css-and-js-file-included-in-inde)

2. 不能使用`webpack-dev-server`作为调试服务器，它不会生成`dist/build.js`， 是直接从内存加载的，没法让webstorm加载。[How to debug Webpack-dev-server (in memory) with WebStorm?
](http://stackoverflow.com/questions/34752270/how-to-debug-webpack-dev-server-in-memory-with-webstorm)

3. Vue文件里的js断点需要打在对应的`dist/build.js`文件里面里面，因为一些vue文件里面的断点没法和`dist/build.js`里面的内容做映射。(可能有更好的解决方法？)

4. 确保编译出来的`dist/build.js`没有压缩过或压缩过变量名, 方便调试。

5. 修改webpack.config.js里
	
		devtool: '#source-map'

6. 添加服务

		  "scripts": {
		    "dev": "cross-env NODE_ENV=development webpack-dev-server --open --hot",
		    "build": "cross-env NODE_ENV=production webpack --progress --hide-modules",
		    "server": "cross-env NODE_ENV=development live-server",
		    "builddev": "cross-env NODE_ENV=dev webpack --progress --hide-modules"
		  },

### 调试

1. 构建`dist/build.js`	
	
		npm run builddev 

2. 启动server

		npm run server

3. 如图配置webstorm
	
	![Configuration in WebStorm](debugging-vue.png?raw=true "Configuration in WebStorm")

4. 在`dist/build.js`里添加断点，开始调试
