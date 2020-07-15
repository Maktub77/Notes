### Ajax			

```js
//封装一个用于发送get/post请求的ajax函数
//参数1：需要请求的服务器地址
//参数2：需要执行的回调函数

function ajax(method, url, data, callback){
    var xhr;
    if(window.XMLHttpRequest){
        xhr = new XMLHttpRequest();
    }else{
        xhr = new ActiveObject('microsoft.XMLHTTP');
    }

    //打开链接
    xhr.open(method, url, true);

    //针对post请求添加请求头
    if(method.toLowerCase() == 'post'){
        xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded")
    }
    //发送请求
    xhr.send(data);
    //当前请求状态发生改变时触发回调函数
    xhr.onreadystatechange = function(){
        if(xhr.readyState == 4 && xhr.status == 200){
            var data = xhr.responseText;
            //			data = JSON.parse(data);
            //闭包
            callback(data);
        }
    }
}
```