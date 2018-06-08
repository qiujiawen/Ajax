# Ajax

#### 环境搭建

第一步服务器的搭建，下载安装Wampserver集成环境。

第二步在服务器环境下模拟Ajax工作原理。

#### Ajax原理：

>ajax是JavaScript用异步的形式操作XML对象。即是通过XmlHttpRequest对象来向服务器发异步请求，从服务器获得数据，然后用javascript来操作DOM而更新页面。

>XMLHttpRequest是ajax的核心机制，就是javascript可以及时向服务器提出请求和处理响应，而不阻塞用户。

>Web的运作原理是一次HTTP请求对应一个页面；而AJAX是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。


#### Ajax过程：

>创建对象XMLHttpRequest()，ie6下创建对象ActiveXObject('Microsoft,XMLHTTP')；

>异步形式打开数据，open();

>发送数据，send()；

>等待服务器返回内容；

>获取内容


#### http常见的状态码:

>200 - 请求成功

>301 - 资源（网页等）被永久转移到其它URL

>404 - 请求的资源（网页等）不存在

>500 - 内部服务器错误

#### 同步异步

>同步：阻塞的，类似串联的灯泡。

>异步：非阻塞的，类似并联的灯泡

#### get和post

>get一般用来进行查询操作，url地址有长度限制，请求的参数都暴露在url地址当中，如果传递中文参数，需要自己进行编码操作，安全性较低。

>post请求方式主要用来提交数据，没有数据长度的限制，提交的数据内容存在于http请求体中，数据不会暴漏在url地址中。


Ajax代码封装示例：

    function ajaxHandle() {
        let xhr = null;

        /**
         *  @description 兼容ie6以下
         */
        try {
            xhr = new XMLHttpRequest();
        } catch (e) {
            xhr = new ActiveXObject('Microsoft,XMLHTTP');
        }

        /**
         *  @description    异步形式打开数据
         *  @param          打开方式
         *  @param          打开地址
         *  @param          是否异步
         *  @code           new Date().getTime()是用于清除缓存
         */

        xhr.open('get','getAjax.php',true);

        //发送数据
        xhr.send();

        /**
         *  @description 等待服务器返回内容
         */
        xhr.onreadystatechange = function () {
            /**
             *  @code readyState是Ajax工作状态，有五个值。
             *    0    （初始化）还没有调用open()方法
             *    1    （载入）已调用send()方法，正在发送请求
             *    2    （载入完成）send()方法完成，已收到全部响应内容
             *    3    （解析）正在解析响应内容
             *    4    （完成）响应内容解析完成，可以在客户端调用了
             */
            if (xhr.readyState === 4) {

                //status：服务器状态，HTTP状态码
                if (xhr.status === 200) {

                    /*
                        ajax请求返回的内容就被存放到这个属性(responseText)下面;
                        responseText是字符串类型；
                        responseText：返回以文本形式存放的内容；
                        responseXML：返回XML形式的内容
                     */
                    let data = JSON.parse(xhr.responseText);

                    console.log(data);
                    let list = document.getElementById('list');
                    let html = '';
                    for (let i = 0;i < data.length;i++){
                        html += '<li><a href="javascript:;"> ' + data[i].title + ' </a>[<span>' + data[i].time + '</span>]</li>';
                    }
                    list.innerHTML = html;
                } else {
                    console.log('出错了，错误信息：' + xhr.status);
                }
            }
        };
    };