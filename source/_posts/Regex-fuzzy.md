---
title: Regex-fuzzy
date: 2018-09-18 00:21:45
tags: [Regex, 正则表达式, vim, fuzzy, 模糊匹配, vscode]
category: [Regex, fuzzy]
---
# 正则表达式
本文以vim为例
### 一，多个式子模糊匹配`[pattern1 pattern2 ...]`
模糊匹配:有一个pattern匹配则匹配成功
- `[a-z]\+`   匹配所有小写英文  
- `[a-zA-Z]`  匹配所有小写，大写英文  
- `[a-zA-Z ]` 匹配所有英文或者空格(中括号里面有空格)    
**注意，在vim中，模糊匹配的中括号里面，匹配空格不可以用\s，匹配字母不可以用\w**  
- `[a-zA-Z \.]`  匹配所有的英文，半角句号，空格

### 二，单个字符模糊匹配
`b\a?` 匹配b或者ba  (vim为例)



### 三应用
##### 1.把if else (condition)替换成case(condition)
原文
```java
if(opera.equals("&")) {
    str = pBin(num1) + " & " + pBin(num2) + " = " + pBin(num1 & num2);
} 
else if (opera.equals("|")){
    str = pBin(num1) + " | " + pBin(num2) + " = " + pBin(num1 | num2);
}
else if (opera.equals("^")){
    str = pBin(num1) + " ^ " + pBin(num2) + " = " + pBin(num1 ^ num2);
}
else if (opera.equals("~")) {
    str = " ~" + pBin(num1) + " = " + pBin(~num1);
}
else if (opera.equals("<<")) {
    str = pBin(num1) + " << " + num2 + " = " + pBin(num1 << num2);
}
else if (opera.equals(">>>")) {
    str = pBin(num1) + " >>> " + num2 + " = " + pBin(num1 >>> num2);
}
```
vim 命令模式下输入
```vim
:%s/\(^[ a-z(\.]\+(\)\("[\&\|\^\~\<\>]\+"\)\([\)]\+\)/case:\2/
```
或者
```vim
:%s/\(^[ a-z.(]\+\)\("[|^~<>]\+"\)\())\)/case:\2/
```
结果
```java
case "&": {
                str = pBin(num1) + " & " + pBin(num2) + " = " + pBin(num1 & num2);
            } 
case "|":{
                str = pBin(num1) + " | " + pBin(num2) + " = " + pBin(num1 | num2);
            }
case "^":{
                str = pBin(num1) + " ^ " + pBin(num2) + " = " + pBin(num1 ^ num2);
            }
case "~": {
                str = " ~" + pBin(num1) + " = " + pBin(~num1);
            }
case "<<": {
                str = pBin(num1) + " << " + num2 + " = " + pBin(num1 << num2);
            }
case ">>>": {
                str = pBin(num1) + " >>> " + num2 + " = " + pBin(num1 >>> num2);
            }
        }
```
改进下，保留开头的空格
```vim
:%s/\( \+\)\([ a-z.(]\+\)\("[|^~<>]\+"\)\())\)/\1case:\3/
```
效果
```java
            case "&": {
                str = pBin(num1) + " & " + pBin(num2) + " = " + pBin(num1 & num2);
            } 
            case "|":{
                str = pBin(num1) + " | " + pBin(num2) + " = " + pBin(num1 | num2);
            }
            case "^":{
                str = pBin(num1) + " ^ " + pBin(num2) + " = " + pBin(num1 ^ num2);
            }
            case "~": {
                str = " ~" + pBin(num1) + " = " + pBin(~num1);
            }
            case "<<": {
                str = pBin(num1) + " << " + num2 + " = " + pBin(num1 << num2);
            }
            case ">>>": {
                str = pBin(num1) + " >>> " + num2 + " = " + pBin(num1 >>> num2);
            }
```
解释

    \( \+\)\([ a-z.(]\+\)\("[|^~<>]\+"\)\())\)/\1case:\3/
    


### 1. `\( 第一组匹配内容 \)  \( 第二组匹配内容 \)  \( ... \)  `  

在这里我们分了三组  
##### 第一组：`\( +\)`
表示匹配开头多个空格，并引用为第一组
##### 第二组：`\([ a-z.(]\+\)`
`[ a-z.(]` 中`a-z`表示匹配的字母范围`a-z`，所有匹配内容引用为第二组
##### 第三组：`\("[|^~<>]\+"\)\())\)`
`[|^~<>]` 表示匹配中括号里面的任意一个字符而后面加上`\+`表示匹配任意一个或者多个字符，匹配内容引用为第三组
    
     
### 2.  `\(  \)  /   替换内容`
符号`/`作为匹配内容和替换内容的分隔
    
### 3.替换内容中的 \1  \2  \3 分别引用第一组匹配内容，第二组，第三组..
应用:

    原文：
        内容1内容2
    替换：
        \(内容1\)\(内容2\)/替换文本a\1替换文本b\2
    结果：
        替换文本a内容1替换文本b内容2
        
相当于在内容1和2后面分别加入替换文本a和b
    
    
    






## 拓展：vscode
`Ctrl + H` 打开替换面板，第一栏输入
```
([ \w(.]+)("[|&^~<>]+")(\)\))
```
第二栏输入
```
case $2 :
```
可以有同样的效果


但是开头的空格没了，改进一下  
  
第一栏
```
(^ +)([\w.( ]+)("[&|~^<>]+")(\)\))
```
第二栏
```
$1case $3:
```
效果
```java
            case "&": {
                str = pBin(num1) + " & " + pBin(num2) + " = " + pBin(num1 & num2);
            } 
            case "|":{
                str = pBin(num1) + " | " + pBin(num2) + " = " + pBin(num1 | num2);
            }
            case "^":{
                str = pBin(num1) + " ^ " + pBin(num2) + " = " + pBin(num1 ^ num2);
            }
            case "~": {
                str = " ~" + pBin(num1) + " = " + pBin(~num1);
            }
            case "<<": {
                str = pBin(num1) + " << " + num2 + " = " + pBin(num1 << num2);
            }
            case ">>>": {
                str = pBin(num1) + " >>> " + num2 + " = " + pBin(num1 >>> num2);
            }
```