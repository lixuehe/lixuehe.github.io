---
layout: post
section-type: post
category: tech
tags: [ 'tutorial' ]
---

# jquery-Ajax

### 一.load()方法异步请求数据

1. **load(url,[data],[callback])**

   * url    :    为加载服务器地址;
   * data    :    参数为请求时发送的数据;
   * callback    :    参数为数据请求成功后，执行的回调函数;

   

### 二．getJSON()方法异步加载JSON格式数据

1. **jQuery.getJSON(url,[data],[callback])**   /
   **$.getJSON(url,[data],[callback]) **
   * url    :    参数为请求加载json格式文件的服务器地址;
   * data    :    参数为请求时发送的数据;
   * callback    :    参数为数据请求成功后，执行的回调函数;

### 三．getScript()方法异步加载并执行js文件

1. **jQuery.getScript(url,[callback])**    /

   **$.getScript(url,[callback])**

   * url    :    为服务器请求地址;
   * callback    :    参数为数据请求成功后，执行的回调函数;

### 四．get()方法以GET方式从服务器获取数据

1. **$.get(url,[callback])**

   * url    :    为服务器请求地址;

   * callback    :    参数为数据请求成功后，执行的回调函数;

     ```
      $(function () {
      	$("#btnShow").bind("click", function () {
      		var $this = $(this);
      	　　$.get(
      	　　"http://www.imooc.com/data/info_f.php",
      	　　function(data) {
              $this.attr("disabled", "true");
              $("ul").append("<li>我的名字叫：" + data.name + "</li>");
              $("ul").append("<li>男朋友对我说：" + data.say + "</li>");
              }, 
              "json");
      	})
      });
     ```

### 五.post()方法以POST方式从服务器发送数据

1. **$.post(url,[data],[callback])**
   * url    :    为服务器请求地址;
   * data    :    参数为请求时发送的数据;
   * callback    :    参数为数据请求成功后，执行的回调函数;

### 六．serialize()方法序列化表单元素值

1. 使用`serialize()`方法可以将表单中有name属性的元素值进行序列化，生成标准URL编码文本字符串，直接可用于ajax请求，它的调用格式如下：

   **$(selector).serialize()**

   * 其中`selector`参数是一个或多个表单中的元素或表单元素本身。

   

   

   ---

   ### 七．ajax()方法加载服务器数据

   1. **jQuery.ajax([settings])**   /   **$.ajax([settings])**

      * settings  :  为发送ajax请求时的配置对象;其中：

        - url表示服务器请求的路径；
        - ｄata为请求时传递的数据;
        - dataType为服务器返回的数据类型;
        - success为请求成功的执行的回调函数;
        - type为发送数据请求的方式，默认为get;

      * ```
        1. url表示服务器请求的路径；
        2. ｄata为请求时传递的数据;
        3. dataType为服务器返回的数据类型;
        4. success为请求成功的执行的回调函数;
        5. type为发送数据请求的方式，默认为get;
        ```

   ### 八．ajaxSetup()方法设置全局Ajax默认选项

   1. **jQuery.ajaxSetup([options])**   /**$.ajaxSetup([options])**

      options参数为一个对象，通过该对象设置Ajax请求时的全局选项值。

   ### 九．ajaxStart()和ajaxStop()方法

   ##### ajaxStart()方法用于在Ajax请求发出前触发函数

   ##### ajaxStop()方法用于在Ajax请求完成后触发函数。它们的调用格式为：

   **$(selector).ajaxStart(function())**   /**$(selector).ajaxStop(function())**

   两个方法中括号都是绑定的函数，当发送Ajax请求前执行`ajaxStart()`方法绑定的函数，请求成功后，执行ajaxStop ()方法绑定的函数。

   

   

   

   

   

   

   

   

   