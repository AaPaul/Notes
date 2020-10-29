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
Ref: 
1. https://blog.csdn.net/qq_42645915/article/details/80967280?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.edu_weight&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.edu_weight
2. https://blog.csdn.net/qq_34888129/article/details/82696171
3. https://blog.csdn.net/qq_39640877/article/details/80566584
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


## Bootstrap
#### Collapse
It controls a collapsed and expanded animation effect.
```[HTML5]
<div class="panel-group">
  {% for option in searchOptions %}
  <div class="panel panel-primary">
    <div class="panel-heading">
      <h4 class="panel-title">
        <a data-toggle="collapse" href="#{{option.alias}}">{{option.name.replace('_',' ').capitalize()}}</a> {# 'data-toggle' is the label of a collapse block and the href corresponds to a block which is recognized by the id. #}
      </h4>
    </div>
    <div id="{{option.alias}}" class="panel-collapse collapse in"> {# the class which includes these 2 parts is to show that this block can be collapsed or expanded and the 'collapse in' means that the default view is expanded#}
      {% if option.options[0].isdigit() and 'year' in option.name %}
      <div class="panel-body">
        <form>
          <div class="form-group form-group-sm input-from-to">
            <input type="text" class="form-control" placeholder="From"> -
            <input type="text" class="form-control" placeholder="To">
            <button type="submit" class="btn btn-primary btn-xs">Update</button>
          </div>
        </form>
      </div>
      {% else %}
      <div class="panel-body">
        {% for value in option.options %}
        <label class="filter-checkbox">
          <input type="checkbox" checked="true" value="{{value}}">{{value}}</label>
        {% endfor %}
      </div>
      {% endif %}
    </div>
  </div>
  {% endfor %
```

#### word wrap <div>
We can set the style by css file or using the internal parameter.
```[HTML5]
.classname {
    width: 200px;
    word-break: break-all; // will influence the completeness of the word.
    word-wrap: break-word;  // will not split the word
}
```
Ref: https://www.bbsmax.com/A/nAJvGynQ5r/

## Others
#### 'GET' vs 'POST'

#### Using 'POST' method to implement downloading function
The reason why not use 'GET' method is because of the limitaion of the length'GET' url on the browser. 
Different browsers has different rule for limiting the length of url. If the length exceeds the limitation, the url would be cut off.
```
IE and Safari: 2k
Opera: 4k
Firefox: 8k
```

```
	function download_data() {
    xhttp = new XMLHttpRequest();
    xhttp.onreadystatechange = function () {
        var a;
        if (xhttp.readyState === 4 && xhttp.status === 200) {
            a = document.createElement('a');
            a.href = window.URL.createObjectURL(xhttp.response);
            a.download = variables.data_default['trait_name']+".csv";
            a.style.display = 'none';
            document.body.appendChild(a);
            a.click()
        }
    };
    xhttp.open("POST", variables.url_downloading); //variables.url_downloading is the url to download
    xhttp.setRequestHeader("Content-Type", "application/json");
    xhttp.responseType = 'blob';
    xhttp.send(JSON.stringify(variables.data_default)); // variables.data_default is the data to download
};
```
Ref: 
- https://stackoverflow.com/questions/26737883/content-dispositionattachment-not-triggering-download-dialog
- https://blog.csdn.net/u010552788/article/details/80593962
- https://www.jianshu.com/p/512389822f8b

#### JSON to str & str to JSON
```
JSON.parse(str)

JSON.stringfy(json)
eval(str)
(new Function("return" + str))()

```
Ref: https://segmentfault.com/a/1190000007368846

#### Get the selected content in the select label
```
$(|"#selection option:selected").val()
```


### DataTable
#### columnDefs
Setting of initializing the attributes of each column
- 0或者正整数-列从左到右是从0开始
- 一个负数-列从右到左的索引(-1代表最后一列)
- 一个字符串-将字符串和类名匹配列
- 字符串"_all"-所有列
```
$('#example').dataTable( {
  "columnDefs": [ {
      "targets": 0,
      "searchable": false
    } ]
} );
```

Ref: http://datatables.club/reference/option/columnDefs.html

#### $('td', row).eq(0)
It means the first block in the first column

`$('tr').eq(0)` means the first row.

#### The issue that the data cannot be renderred into the table.
In the part of initializing table, we can allocate the data for each column by using `$('td', row).eq(num).html('content')`.