---
title: ajax-jq封装
date: 2016-9-16
---

# ajax-jq封装
* 利用jq封装ajax
```javascript
var $={
    ajax:function(obj){
         var xhr=new XMLHttpRequest();
        //请求发送之前调用.
         var flag=obj.beforeSend();
         if(flag==false){
             return;
         }   
         if(typeof obj.data==='object'){
             //把对象转换成字符串.
             var str=this.params(obj.data);
             obj.data=str;
         }
        if(obj.type.toLocaleLowerCase()=="get"){
             //data 可能是一个对象
             //我要把对象变成字符串
             obj.url=obj.url+"?"+obj.data;
             obj.data=null;
         }
         xhr.open(obj.type,obj.url);
         if(obj.type.toLocaleLowerCase()=="post"){
             xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
         }
         xhr.send(obj.data);
         xhr.onreadystatechange=function(){
              if(xhr.readyState==4){
                   if(xhr.status==200){
                       var data=xhr.responseText;
                       //请求成功的时候调用.
                      var jsonobj=JSON.parse(data);
                       obj.success(jsonobj);
                   }else{
                       //代表请求失败的时候调用
                       obj.error();
                   }
                   //请求完成的时候调用.
                   obj.complete();
              }
         }
    },
    //将一个对象转换成一个字符串
    //username=zhangsan&age=11
    params:function(obj){

         /*
         *{
          username:"lisi",
          age:11,
          sex:"nan"
          }
         *转换成username=zhangsan&age=11
         * */
         var str="";
         for(var key in obj){
             //console.log(key);
             str+=key+"="+obj[key]+"&"
         }

        console.log(str);
        str=str.substr(0,str.length-1);

        console.log(str);

        return str;
    }
}
```
