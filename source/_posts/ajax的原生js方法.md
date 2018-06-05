---
title: ajax的原生js方法
date: 2018-06-05 18:05:00
tags: [ ajax, javascript ]
---

### html代码

```
<link rel="stylesheet"
      href="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.12.0/styles/default.min.css">
<script src="//cdnjs.cloudflare.com/ajax/libs/highlight.js/9.12.0/highlight.min.js"></script>
   <script>
        document.getElementById("search").onclick = function() {
            var request = new XMLHttpRequest();
            request.open("GET", "test.php?number=" + document.getElementById("keyword").value);
            request.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
            request.send();
            request.onreadystatechange = function() {
                if (request.readyState == 4 && request.status == 200) {
                    document.getElementById("searchResult").innerHTML = request.responseText;
                }
            }
        }
        document.getElementById("save").onclick = function() {
            //sent request;
            //================发送查询请求并且处理=================//
            var request = new XMLHttpRequest();
            //发送请求
            //request.open("GET","test.php?number="+document.getElementById("keyword").value);
            //GET方法的传递变量是直接在php后面?<变量>+
            request.open("POST", "test.php");
            var data = "name=" + document.getElementById("staffName").value +
                "&number=" + document.getElementById("staffNumber").value +
                "&sex=" + document.getElementById("staffSex").value +
                "&job=" + document.getElementById("staffJob").value;

            //重要:
            request.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
            //POST方法传递变量用字符串储存，变量之间用&隔开
            request.send(data);
            //
            request.onreadystatechange = function() {
                if (request.readyState === 4) {
                    if (request.status == 200) {
                        //查询结果
                        //document.getElementById("searchResult").innerHTML=request.responseText;
                        //创建结果
                        document.getElementById("createResult").innerHTML = request.responseText;
                    } else {
                        alert("发生错误：" + request.status);
                    }
                }
            }
        }
    </script>
```

### php代码
```
<?php
header("Content-Type:text/plain;charset=utf-8");

$staff=array(
    array("name" => "hong7", "number" => "101", "sex" => "male", "job" => "manager"),
    array("name" => "guojing", "number" => "102", "sex" => "female", "job" => "扫地"),
    array("name" => "uangron", "number" => "103", "sex" => "male", "job" => "打杂"),
);

if($_SERVER["REQUEST_METHOD"]=="GET") {
    search();
} elseif ($_SERVER["REQUEST_METHOD"]=="POST") {
    create();
}

function search() {
    if(!isset($_GET["number"]) || empty($_GET["number"])) {
        echo "syntax error hahaha";
        return;
    }
    global $staff;
    $number = $_GET["number"];
    $result = "no such people";

    foreach($staff as $value) {
        if ($value["number"] == $number) {
            $result = "find: \nnumber: " . $value["number"]. "\nname: ".
            $value["name"] . "\nsex: ". $value["sex"] . "\nJob: " . $value["job"];
            break;
        }
        
    }
    echo $result;
}



function create() {
    if(
        (!isset($_POST["name"]) || empty($_POST["name"]))
        ||(!isset($_POST["number"]) || empty($_POST["number"]))
        ||(!isset($_POST["sex"]) || empty($_POST["sex"]))
        ||(!isset($_POST["job"]) || empty($_POST["job"]))
    ) {
        echo "syntax error , people info not enough";
        return;
    } else {
    echo "员工 :" . $_POST["name"]. " saved !";
    }
}



?>
```