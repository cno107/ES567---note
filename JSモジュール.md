# JSモジュール

## 前書き

1. 什么是模块/模块化?
  将一个复杂的程序依据一定的规则(规范)封装成几个块(文件), 并进行组合在一起
  块的内部数据/实现是私有的, 只是向外部暴露一些接口(方法)与外部其它模块通信
2. 模块化的好处
  　　　避免命名冲突(减少命名空间污染)
    　　　更好的分离, 按需加载
    　　　更高复用性
    　　　高可维护性
3. 为什么要モジュール

+ web sites are turning into Web Apps

+ Code complexity grows as the site gets bigger
+ Highly decoupled JS files/modules is wanted
+ Deployment wants optimized code in few HTTP calls

4. 页面引入加载script

问题
	请求过多
	依赖模糊
	难以维护

## CommonJS

+ 说明

​                         http://wiki.commonjs.org/wiki/Modules/1.1

每个文件都可当作一个模块

- 在服务器端: 模块的加载是运行时同步加载的

+ 在浏览器端: 模块需要提前编译打包处理

+ 基本语法  暴露模块

​         module.exports = value

​          exports.xxx = value

+ 问题: 暴露的模块到底是什么?

+ 引入模块         require(xxx)

​                第三方模块:xxx为模块名

​                自定义模块: xxx为模块文件路径

### 服务器端实现

Node.js       http://nodejs.cn/

```javascript
module.exports = () => {
    console.log('module2');
}
```

### 浏览器端实现

Browserify

http://browserify.org/

也称为CommonJS的浏览器端的打包工具

区别Node与Browserify

Node.js运行时动态加载模块(同步)

Browserify是在转译(编译)时就会加载打包(合并)require的模块

####Browserify模块化

1. 创建项目结构
  ```
  |-js
    |-dist //打包生成文件的目录
    |-src //源码所在的目录
      |-module1.js
      |-module2.js
      |-module3.js
      |-app.js //应用主源文件
  |-index.html
  |-package.json
    {
      "name": "browserify-test",
      "version": "1.0.0"
    }
  ```
2. 下载browserify  **两次都需要**
  * 全局: npm install browserify -g        (root下install)
  * 局部: npm install browserify --save-dev
3. 定义模块代码
  * module1.js
    ```
    同node.js
    ```
  * app.js (应用的主js)
    ```
    同node.js
    ```
* **打包处理js**:

  * **browserify   js/src/app.js     -o       js/dist/bundle.js**
  * ​     命令              source      output To       target
* 页面使用引入:
  ```
  <script type="text/javascript" src="js/dist/bundle.js"></script> 
  ```

## AMD

### NoAMD

+ dB.js 无依赖

```javascript
+function (window) {
    let name = 'b.js';
    function getName() {
        return name;
    }
    window.b = {getName};
}(window)
```

+ aL.js 有依赖

```javascript
+function (window,b) {
  let msg = 'a.js';
  function showMsg() {
      console.log(msg,b.getName());
  }
  window.a = {showMsg};
}(window,b)
```

+ App.js 导出js

```javascript
+function (a) {
    a.showMsg();
}(a)
```

+ html中使用

```html
<script src="js/dB.js"></script>
<script src="js/aL.js"></script>
<script src="app.js"></script>
```

### RequireJS

1. 下载require.js, 并引入
  * 官网: http://www.requirejs.cn/
  * github : https://github.com/requirejs/requirejs
  * 将require.js导入项目: js/libs/require.js 

2. 创建项目结构
  ```
  |-js
    |-libs
      |-require.js
    |-modules
      |-a.js
      |-b.js
    |-main.js
  |-index.html
  ```

3. 定义require.js的模块代码
  * b.js
    ```
    define(function () {
      let msg = 'atguigu.com'
    
      function getMsg() {
        return msg.toUpperCase()
      }
    
      return {getMsg}
    })
    ```
  * a.js
    ```
    define(['dataService', 'jquery'], function (dataService, $) {
      let name = 'Tom2'
    
      function showMsg() {
        $('body').css('background', 'gray')
        alert(dataService.getMsg() + ', ' + name)
      }
    
      return {showMsg}
    })
    ```

4. 应用主(入口)js: main.js
  ```javascript
  (function () {
    //配置
    requirejs.config({
      //基本路径
      baseUrl: "js/",
      //模块标识名与模块路径映射
      paths: {
        "alerter": "modules/alerter",
        "dataService": "modules/dataService",
      }
    })
    
    //引入使用模块
    requirejs( ['alerter'], function(alerter) {
      alerter.showMsg()
    })
  })()
  ```

5. 页面使用模块:

  ```html
  <script data-main="js/main" src="js/libs/require.js"></script>
  ```
------------------------------------------------------------------------

6. 使用第三方基于require.js的框架(jquery)
  * 将jquery的库文件导入到项目: 

    * js/libs/jquery-1.10.1.js

  * 在main.js中配置jquery路径
    ```javascript
    paths: {
              'jquery': 'libs/jquery-1.10.1'    //必须是小写
          }
    ```

    ```javascript
    //jquery-3.3.1.js
    if ( typeof define === "function" && define.amd ) {
       define( "jquery", [], function() {   //这里说了是小写
          return jQuery;
       } );
    }
    ```

    

  * 在alerter.js中使用jquery
    ```javascript
    define(['dataService', 'jquery'], function (dataService, $) {
        var name = 'xfzhang'
        function showMsg() {
            $('body').css({background : 'red'})
            alert(name + ' '+dataService.getMsg())
        }
        return {showMsg}
    })
    ```
------------------------------------------------------------------------

7. 使用第三方不基于require.js的框架(angular)
    * 将angular.js导入项目
    * js/libs/angular.js

  * 在main.js中配置
    ```javascript
    (function () {
      require.config({
         baseUrl: "js/",
       paths: {
          //第三方库
          'jquery' : './libs/jquery-1.10.1',
          'angular' : './libs/angular',
          //自定义模块
          "alerter": "./modules/alerter",
          "dataService": "./modules/dataService"
        },
        /*
         配置不兼容AMD的模块
         exports : 指定与相对应的模块名对应的模块对象
         */
        shim: {
          'angular' : {
            exports : 'angular'
          }
        }
      })
      //引入使用模块
      require( ['alerter', 'angular'], function(alerter, angular) {
        alerter.showMsg()
        console.log(angular);
      })
    })()
    ```

## ES6

###export

1. 分多次导出模块的多个部分
   export class Emp{  }
   	export function fun(){  }
   	export var person = {};

2. 一次导出模块的多个部分
   class Emp{  }
   	function fun(){  }
   	var person = {};
   	export {Emp, fun, person}

3. default导出(只能有一个)

   ```javascript
   export default {       //导入默认的    而且有且只有一个
     name: 'Tom',
     setName: function (name) {
       this.name = name
     }
   }
   ```

###import

```javascript
//es6 解构赋值的形式去拿值  分割代入 (Destructuring assignment)
//中间那玩意是变量
import defaultModule from './myModule';  //导入默认的
import {Emp} from './myModule'; //导入指定的一个
import {Emp, person} from './myModule'; //导入指定的多个
import * as allFromModule from './myModule';  //导入所有
```

###Babel-Browserify使用教程

1. 定义package.json文件
  ```
  {
    "name" : "es6-babel-browserify",
    "version" : "1.0.0"
  }
  ```

2. 安装babel-cli, babel-preset-es2015和browserify   
  * npm install babel-cli browserify -g  

  ```javascript
  //cli : command line interface
  ```

   * npm install babel-preset-es2015 --save-dev 
   * preset 预设(将es6转换成es5的所有插件打包)

3. 定义```.babelrc```文件 
   ```json
   {                           //rc  ： run control
    "presets": ["es2015"]     //用来告诉babel 接下来要处理es2015
     }
   ```

4. 编码
  * js/src/module1.js  分别暴露
    ```javascript
    export function foo() {
      console.log('module1 foo()');
    }
    export const DATA_ARR = [1, 3, 5, 1]
    ```
  * js/src/module2.js  统一暴露
    ```javascript
    let data = 'module2 data'
    
    function fun1() {
      console.log('module2 fun1() ' + data);
    }
    export {fun1, fun2}
    ```
  * js/src/module3.js
    ```javascript
    export default {       //导入默认的    而且有且只有一个
      name: 'Tom',
      setName: function (name) {
        this.name = name
      }
    }
    ```
  * js/src/app.js
    ```javascript
    import {foo, bar} from './module1'
    import {DATA_ARR} from './module1'
    import {fun1, fun2} from './module2'
    import person from './module3'
    
    import $ from 'jquery'
    $('body').css('background', 'red')
    foo()
    console.log(DATA_ARR);
    fun2()
    person.setName('JACK')
    console.log(person.name);
    ```

5. **编译**

  * 使用Babel将ES6编译为ES5代码(但包含CommonJS语法) :

  ```javascript
   babel js/src -d js/lib
   //左边是要编译的文件夹   -d    右边输出的文件夹(不存在就自动创建)
  ```

  * 使用Browserify编译为浏览器可识别的js :

  ```javascript
   browserify js/lib/main.js -o js/lib/bundle.js
  //仅仅编译 babel编译后的出口文件
  ```

6. 页面中引入测试
  ```
  <script type="text/javascript" src="js/lib/bundle.js"></script>
  ```

7. 引入第三方模块(jQuery)
  1). 下载jQuery模块: 
    * npm install jquery@1 --save
    2). 在app.js中引入并使用
    ```
    import $ from 'jquery'
    $('body').css('background', 'red')
    ```





