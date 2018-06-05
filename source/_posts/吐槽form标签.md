---
title: 吐槽form标签
date: 2018-06-06 02:55:30
tags: [ajax, html]
---
### form 标签
今天在练习ajax使用方法的时候，把所有的input 和其他div标签放在form标签里面
```
    <form>
        <span>学号：</span><input type="text" id="number" /> <br>
        <span>密码：</span><input type="password" id="password" /> <br>
        <button id="submit">button</button>
        <div id="status">结果</div>
    </form>
```
结果php文件传输过来的数据瞬间消失，ajax文件如下
```
 <script>
        $(document).ready(function() {
            $("#submit").click(function() {
                $.ajax({
                    type: "POST",
                    url: "search-score.php",
                    dataType: "json",
                    data: { //其属性没有双引号
                        number: $("#number").val(),
                        password: $("#password").val()
                    },
                    success: function(data) {
                        if (data.success) {
                            $("#status").html(data.msg);
                        } else {
                            $("#status").html = "出现错误: " + data.msg;
                        }
                    },
                    error: function(jqXHR) {
                        alert("发生错误：" + jqXHR.status);
                    }
                });
            })
        })
</script>
```


### JSON 格式挺坑的
这里贴上几个可行的json对象，方便以后参考吧
#### 单行
```
'{"success" : true, "msg": "hello" }'

$result = '{"success": true, "msg": "'.$number.':该学号不存在"}';
 $result = '{"success": true, "msg": "'.$_POST["password"].$number.':该学号不存在"}';
```
#### 多行
```
$result = '{
                "success":true,
                "msg": "find: number:'.$value["number"].
                            ' name: '.$value["name"].
                            ' sex: ' .$value["sex"].
                            ' job: ' .$value["job"].'"
            }';
```
```
 $result = '{
     "success": true,
      "msg": "'
            .$_POST["password"]
            .$number
            .':该学号不存在"
            }';
 ```

```
$result = '{
        "success": true,
            "msg": 
                    "'                           //空字符串
                    .$_POST["password"]         //连接第一个字符串
                    .$number                    //连接第二个字符串
                    .':该学号不存在'             //连接第四个字符串
                    .'哈哈哈"                    
                }';
```
```
$result = '{
        "success": true,
            "msg": "字符串0'.$_POST["password"].$number.'字符串1'.'字符串2"                    
                }';
```
#### php中json格式的连接字符串书写注意


- 字符串之间拼接：首个字符串string1和最后一个字符串string4的外侧必须是双引号，内测必须是单引号，且用`.`连接
        
        "key" : "string1'.'string2'.'string3'.string4"



- 数字拼接成字符串：两边的``"'``为空字符串，中间的数字直接用`.`连接，不需要单引号

        "key" : "'.$value1.$value2.$value3.'"


- 字符串与数字：字符串用单引号引起来，再与数字用`.`连接，最外层两边的的注意事项同上面两个

        "key" : "string1'.$value.'string2'.$value.'"



### 总结
- 使用ajax时，我们应该把form标签去掉，否则无法显示后台传输的数据，具体原因也不清楚
- 字符串的连接需要多加练习


### 说多都是泪啊