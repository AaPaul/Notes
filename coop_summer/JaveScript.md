## JS HTML DOM
#### window.location.href


## AJAX
#### Basic 
```[javascript]
$.ajax({
    method:
    type: 
    data:
    url:
    dataType:
})
```

#### The way to transfer the data to server from front-end.
Set the contentType(datatype) to "application/json; charset=utf-8" and then set data using JSON.stringify({"key": value})
```
$.ajax({
        url : "../../admin/insert/user",
        type : 'post',//method请求方式，get或者post
        dataType : 'json',//预期服务器返回的数据类型
        contentType : "application/json; charset=utf-8",
        data :JSON.stringify(data.field) ,
        success : function(result) {
            if (result == "success") {
                        console.log("添加成功")
            } else {
                console.log("添加失败")
            }
        },
        error : function(msg) {
        }
    })
```

## LocalStorage
#### LocalStorage.setItem("name", value)
To store the value with the name in local storage.

#### LocalStorage.getItem("name")
Get the value

#### LocalStorage.removeItem("name")
It is easy to check the data from the front-end by localstorage functions and to make sure the confidentiality, we can use `removeItem` to clear the data.

https://blog.csdn.net/fd214333890/article/details/48809061
Function summarization: https://blog.csdn.net/gao_xu_520/article/details/79128231 

## jQuery
#### Basic syntax
```[javascript]
$(selector).action()
```
`'#test'` means the element with id 'test'.
**Tips:** If you use the jQuery online like from CDN, Microsoft and google, etc, it will improve the loading efficiency.


## Others
#### 'GET' vs 'POST'
