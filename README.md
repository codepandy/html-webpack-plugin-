# html-webpack-plugin-demo
html-webpack-plugin插件的功能简单说就一句话：“生成html文件”

### 场景描述
* 现在有多个html文件```main.html```、```work.html```,各自引用的js文件不同，```main.js,work.js```
* 在这种情况下，如何打包？
* 因大多教程讲解的就是webpack打包是把所有文件打包成一个文件，
* 但是很多项目开发，不是提供一个统一的js入口（react是这样的），各自有各自的js文件，
* 所以在这种情况下把所有的js打包成一个文件，显然是不合适的
* 合适的场景是每个html只是打包自己的引用的js，这样不会使每个页面加载不想关的js
* 因此就需要这个插件来帮你完成你的功能

**本demo不讲解基本的环境搭建，比如安装什么的**

### 下面是webpack.config.js的配置文件

```
var HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
    entry: {
        main: __dirname + '/app/main.js',
        work: __dirname + '/app/work.js'
    },
    output: {
        path: __dirname + '/deploy',
        filename: '[name].bundle.min.js',
        //chunkFilename: '[id].bundle.js'//chunkname我的理解是未被列在entry中，却又需要被打包出来的文件命名配置。什么场景需要呢？我们项目就遇到过，在按需加载（异步）模块的时候，这样的文件是没有被列在entry中的，如使用CommonJS的方式异步加载模块：
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: './app/main.html',//相对于根目录
            filename: 'main.html',
            title: 'plugin title',
            inject: true,
            hash: true,
            chunks: ['main']
        }),
        new HtmlWebpackPlugin({
            template: './app/work.html',//相对于根目录
            filename: 'work.html',
            title: 'work title',
            inject: true,
            hash: true,
            chunks: ['work']
        }),
    ]
}
```

### 参数讲解
* entry

```
    entry: {
        main: __dirname + '/app/main.js',
        work: __dirname + '/app/work.js'
    }
 ```
 这个是entry属性的完整定义，直接写一个字符串只是简写，对象中定义的属性名```mian```和```work```将会在下面chunks中用到
 
 * plugins
 
 ```heml-webpack-plugin```是一个插件，所以需要在plugins中配置
 
 ```
     plugins: [
        new HtmlWebpackPlugin({
            template: './app/main.html',//相对于根目录
            filename: 'main.html',
            title: 'plugin title',
            inject: true,
            hash: true,
            chunks: ['main']
        }),
        new HtmlWebpackPlugin({
            template: './app/work.html',//相对于根目录
            filename: 'work.html',
            title: 'work title',
            inject: true,
            hash: true,
            chunks: ['work']
        }),
    ]
```
>如果多个页面，需要```new```多个```HtmlWebpackPlugin```实例

#### html-webpack-plugin参数讲解
* template

顾名思义，就是设置生成html文件依据的模板，如果你没用模板，这个就应该是你对应的html页面，比如该demo中的```main.html,work.html```
  
* filename

生成html文件的名字，比如原来的```mian.html```生成的名字还是这个，这个也可以加些其他参书，

比如```hash```值```filename: '[hash].main.html',```

* title
生成html文件的title属性的值
* inject
inject
注入选项。有四个选项值 true, body, head, false.
  * true 默认值，表示启用注入，启用注入时，默认是script标签位于html文件的 body 底部，也就是和设置为```body```一样
  * body同 true
  * head script 标签位于 head 标签内
  * false 不插入生成的 js 文件，只是单纯的生成一个 html 文件

* hash 在script引用链接后面生成一个hash值
```
<script type="text/javascript" src="main.bundle.min.js?b9203d842146348b9b35"></script></body>
```

* chunks 引入的模块，这里指定的是entry中设置多个js时，在这里指定引入的js，如果不设置则默认全部引入,模块的名称就是entry中定义的key，比如demo中的```main,work```
chunks来设置生成的html引用的哪些js

**参考链接**

* [https://segmentfault.com/a/1190000007294861](https://segmentfault.com/a/1190000007294861) 
* [http://www.cnblogs.com/haogj/p/5160821.html](http://www.cnblogs.com/haogj/p/5160821.html) 
* [http://www.cnblogs.com/wonyun/p/6030090.html](http://www.cnblogs.com/wonyun/p/6030090.html) 
* [http://react-china.org/t/webpack-output-filename-output-chunkfilename/2256/2](http://react-china.org/t/webpack-output-filename-output-chunkfilename/2256/2) 
* [https://segmentfault.com/a/1190000008590102](https://segmentfault.com/a/1190000008590102)
