# \[js\]ajax

**注：**ajax请求的测试需要配合后端数据，同时需要`http`的相关知识和查看`network`控制台的能力

## 简介

​ Asynchronous JavaScript And XML效果：不刷新整个页面的情况下发送HTTP请求，实现局部刷新

​ 使用场景：账号密码检验、搜索输入框中的补全、三级地址、懒加载

​ 实例：`XMLHttpRequest`、在浏览器的开发者工具中的名称是：`XHR`

​ XML已被弃用：json格式更容易使用JavaScript原生API操作

​ ajax的缺点：

```text
        1、存在跨域问题、
```

​ 2、没有浏览历史（即无法撤销操作）、

​ 3、SEO不好【首次抓取的内容是不包含ajax加载的内容】

## `XMLHttpRequest`

```javascript
            // 1、创建对象
            const xhr = new XMLHttpRequest();
            // 2、初始化 设置参数1：请求方法参数2：url 参数3：是否异步
            xhr.open('GET', 'http://127.0.0.1:8000/server?username=zoulam&password=123456');
            // 3、发送
            xhr.send();
            // 4、事件绑定 处理服务端返回的结果

            /*           on:当……时
                         readystate 就绪状态 xhr对象中的属性
                         有五个表示状态的值：
                            0 未初始化
                            1 open执行
                            2 send执行
                            3 服务端返回部分结果
                            4 服务端返回全部结果
                         change 改变 */

            xhr.onreadystatechange = function () {
                // 判断请求是否完全拉取
                if (xhr.readyState === 4) {
                    // 判断响应状态码
                    if (xhr.status >= 200 && xhr.status < 300) {
                        // console.log(xhr.status);// 状态码
                        // console.log(xhr.statusText);// 状态字符串
                        // console.log(xhr.getAllResponseHeaders());//所有响应头
                        // console.log(xhr.response);// 响应体 一般返回的是json对象
                        ans.innerHTML = xhr.response;
                    }
                }
            }
```

## 常见问题

### ie缓存问题

```javascript
            // 每次的时间不同就会被认为是不同的请求，就不会走缓存
            xhr.open('GET', 'http://127.0.0.1:8000/ie?t='+Date.now());
```

### 网络超时和异常（`xhr.ontimeout`）

```javascript
            const xhr = new XMLHttpRequest();
            xhr.timeout = 2000;
            xhr.ontimeout=function(){
                alert('当前网络状态不佳，请稍后重试')
            }
            xhr.onerror=function(){
                alert('无网络连接')
            }
            xhr.open('GET', 'http://127.0.0.1:8000/outtime');
            xhr.send();
            xhr.onreadystatechange = function () {
                if (xhr.readyState === 4 && xhr.status >= 200 && xhr.status < 300) {
                    ans.innerHTML = xhr.response;
                }
            }
```

### 取消请求（`xhr.abort()`）

```javascript
        const btn = document.querySelectorAll('button');
        let x = null;
        btn[0].onclick = function () {
            x = new XMLHttpRequest();
            x.open('GET', 'http://127.0.0.1:8000/outtime')
            x.send();
        }
        btn[1].onclick = function () {
            x.abort();
        }
```

### 重复请求

```javascript
        const btn = document.querySelectorAll('button');
        let x = null;
        let isSending = false;
        btn[0].onclick = function () {
            // 上一条请求未完成则取消请求
            if(isSending) x.abort();
            x = new XMLHttpRequest();
            isSending = true;
            x.open('GET', 'http://127.0.0.1:8000/outtime')
            x.send();
            x.onreadystatechange = function () {
                if (x.readyState === 4){
                    isSending = false;
                }
            }
        }
        btn[1].onclick = function () {
            x.abort();
        }
```

## Jquery的实现

> 更多内容请查阅官网

```javascript
        $('button').eq(0).click(function(){
            $.get('http://127.0.0.1:8000/jquery-server', {a:100, b:200}, function(data){
                console.log(data);
            },'json');
        });

        $('button').eq(1).click(function(){
            $.post('http://127.0.0.1:8000/jquery-server', {a:100, b:200}, function(data){
                console.log(data);
            });
        });

        $('button').eq(2).click(function(){
            $.ajax({
                //url
                url: 'http://127.0.0.1:8000/jquery-server',
                //参数
                data: {a:100, b:200},
                //请求类型
                type: 'GET',
                //响应体结果
                dataType: 'json',
                //成功的回调
                success: function(data){
                    console.log(data);
                },
                //超时时间
                timeout: 2000,
                //失败的回调
                error: function(){
                    console.log('出错啦!!');
                },
                //头信息
                headers: {
                    c:300,
                    d:400
                }
            });
        });
```

## axios的实现

> 更多内容请查阅官网

```javascript
 const btns = document.getElementsByTagName('button');
        axios.defaults.baseURL = 'http://127.0.0.1:8000'
        btns[0].onclick = function () {
            axios.get('/axios-server', {
                // url 参数
                params: {
                    id: 'zoulam',
                    age: 18
                },
                header: {
                    name: 'luluxi',
                    age: 16
                }
            }).then(value => {
                console.log(value);
                console.log(value.data);
            });;
        }

        btns[1].onclick = function () {
            axios.post('/axios-server',
                // 体
                {
                    username: 'zoulam',
                    password: '123456',
                }, {
                // url
                params: {
                    id: 12,
                    vip: 6
                },
                // 头
                headers: {
                    dd: 'cat',
                    age: 15
                }
            })
        }

        btns[2].onclick = function () {
            axios({
                method: 'POST',
                url: '/axios-server',
                params: {
                    vip: 10,
                    level: 30
                },
                headers: {
                    a: 100,
                    b: 200
                },
                data: {
                    username: 'zoulam',
                    password: '123456'
                }
            }).then(value => {
                console.log(value);
                console.log(value.status);
                console.log(value.statusText);
                console.log(value.headers);
                console.log(value.data);
            });
        }
```

## 新的发送ajax请求的方案`fetch`

> 该方法就在全局，可以直接使用

[more](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API)

```javascript
            fetch('http://127.0.0.1:8000/fetch-server',{
                method:'POST',
                headers:{
                    name:'zoulam'
                },
                body:'usernmme=zoulam&password=123456'
            }).then(value => {
                // return value.text();
                return value.json();
            }).then(value => {
                console.log(value);
            });
```

