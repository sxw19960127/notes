# 1.概述

**html**：`Hyper Text Markup Language` 超文本标记语言

**css**：`Cascading Style Sheet` 层叠样式表



# 2.编码字符集的发展历程

```
gb2312: 国家标准第2312条,识别不了台湾地区的繁体字符。
gbk: 国家版本扩展版本,包括所有亚裔字符集、繁体字符集。
unicode: 万国码。
UTF-8: 万国码升级版本。
```



# 3.主流浏览器

概述：在市场上有一定的份额，且必须有独立研发的内核。

浏览器的组成：`shell`外壳 & 内核（影响着浏览器运转快不快，识别操作运行代码的能力）

|  主流浏览器   |     内核      |
| :-----------: | :-----------: |
|      IE       |    trident    |
|    Firefox    |     gecko     |
| Google Chrome | webkit / bink |
|    Safari     |    webkit     |
|     Opera     |    presto     |



# 4.标签类型

- 根标签：`<html></html>`
- 结构标签：`<head></head>`、`<body></body>`
- 段落标签：`<p></p>`
- 标题标签：`<h1></h1>`
- 加粗标签：`<strong></strong>`
- 倾斜标签：`<em></em>`
- 中划线标签：`<del>$3</del>`
- 空格显示标签：`&nbsp;`
- 回车标签：`<br>`
- 分割线标签：`<hr>`
- 容器功能标签：`<div></div>`、`<span></span>`
- `<`：`&lt;`
- `>`：`&gt;`



# 5.元素类型

- 块元素

概述：独占一行，可以设置宽高（默认100%）。

文字类块元素：`p`、`h1~h6`

容器类块元素：`div`、`table`、`tr td th`、`form`、`dl dt dd`、`ul li`、`ol`

- 行元素

概述：不独占一行，不可以设置宽高（由内容决定）。

`a`、`img`、`input`、`span`、`strong`、`em`、`del`

- 行级块元素

大小由内容决定，可以改变宽高。

`img`

---

规则：块元素可以嵌套任何元素（文字类块元素里不能嵌套块元素），行元素只能嵌套行元素。



# 6.`Form`表单

用途：收集用户输入，发送提交给服务器。

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
</head>
<body>
   <!-- action: 控制提交给服务器的是哪一个网址 -->
   <!-- method: 提交表单的方式(默认为get提交) -->
   <form action="" method="">
      用户名：<input type="text" name="user"><br>
      密码：<input type="password" name="password"><br>
      <!-- 通过 ?user=sxw&password=123 的形式将提交信息添加在URL地址后面 -->
      <input type="submit" value="提交">
   </form>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
</head>
<body>
   <form action="" method="">
      <!-- radio: 单选模式。 -->
      <!-- name属性值必须相同才能起到单选的目的。 -->
      <!-- 提交结果 ?sex=female -->
      <input type="radio" name="sex" value="male" checked>男
      <input type="radio" name="sex" value="female">女
      <input type="radio" name="sex" value="Unknown">外星人

      <input type="submit" value="提交">
   </form>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
</head>
<body>
   <!-- checkbox: 多选模式。 -->
   <!-- 提交结果 ?ckb=chifan&ckb=wan -->
   <form action="" method="">
      <input type="checkbox" name="ckb" value="kanshu" checked>看书
      <input type="checkbox" name="ckb" value="chifan">吃饭
      <input type="checkbox" name="ckb" value="wan">玩

      <input type="submit" value="提交">
   </form>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
</head>
<body>
   <!-- select: 下拉框 -->
   <!-- 提交结果: ?city=beijing -->
   <form action="" method="">
      <select name="city" id="">
         <option value="shanghai">上海</option>
         <option value="beijing">北京</option>
      </select>

      <input type="submit" value="提交">
   </form>
</body>
</html>
```

**文本框事件**：

- `onblur`：当失去输入焦点后触发该事件。
- `onfocus`：当获得输入焦点后触发该事件。
- `onchange`：当文字值发生改变，且在失去焦点后触发该事件。
- `onselect`：当选中输入框文字后，触发该事件。

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
</head>
<body>
   <input type="text" onblur="show1()" onfocus="show2()" onchange="show3()" onselect="show4()">
   <script>
      function show1() {
         console.log('我现在失去输入焦点了')
      }
      function show2() {
         console.log('我现在获得输入焦点了')
      }
      function show3() {
         console.log('当前输入框正在被改变')
      }
      function show4() {
         console.log('我选中文字了')
      }
   </script>
</body>
</html>
```



# 7.`Table`表格

- 行：`<tr>`
- 标题：`<th></th>`
- 内容：`<td></td>`
- 列数合并：`colspan`
- 行数合并：`rowspan`

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <style>
      table, tr, th, td{
          border: 1px solid #000;
      }
  </style>
</head>
<body>
   <table> <!--table下-->
      <tr> <!--行-->
          <th>部门</th> <!--th标题-->
          <th>姓名</th>
          <th>性别</th>
          <th>工资</th>
      </tr>
      <tr>
          <td rowspan="4">开发组</td> <!--td内容,rowspan合并3组内容成1种开发组的内容,下文要删除对应td-->
          <td>小明</td>
          <td>男</td>
          <td>1W</td>
      </tr>
      <tr>
          <!-- <td>开发组</td> -->
          <td>小宏</td>
          <td>女</td>
          <td>1W</td>
      </tr>
      <tr>
          <!-- <td>开发组</td> -->
          <td>小刚</td>
          <td>男</td>
          <td>1W</td>
      </tr>
      <tr>
          <!-- <td>开发组</td> -->
          <td colspan="2">工资合计</td>
          <td>3W</td>
      </tr>
  </table>
</body>
</html>
```



# 8.`css`权重

|         css权重         | 256进制的数 |
| :---------------------: | :---------: |
|       ! important       |  Infinity   |
|        行间样式         |    1000     |
|           id            |     100     |
| class、属性、伪类选择器 |     10      |
|   标签、伪元素选择器    |      1      |
|         通配符          |      0      |



# 9.选择器

|     id 选择器      |
| :----------------: |
|  **class 选择器**  |
|   **标签选择器**   |
|  **通配符选择器**  |
|   **属性选择器**   |
|   **父子选择器**   |
|   **伪类选择器**   |
| **相邻同级选择器** |



# 10.样式规则

- 优先原则：后解析覆盖前解析。
- 继承原则：嵌套里面的标签拥有外部标签的某些样式，子元素可以继承父元素的某些属性。
- 外部样式：与内部样式合并之后一起解析。先解析外部样式，再解析内部样式。
- 内联样式：外部和内部样式都解析完成之后，开始解析内联样式。
- `!important`字段的样式最后解析。



# 11.继承原则

- 和文字样式相关的可被继承

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
</head>
<body>
   <!-- div1、div2、p1、p2都变红 -->
   <div style="color: red;">div1
      <div>div2
         <p>p1</p>
      </div>
      <p>p2</p>
   </div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
</head>
<body>
   <!-- 只有div1有效果,后续子元素不能被继承 -->
   <div style="border: 1px solid black;">div1
      <div>div2
         <p>p1</p>
      </div>
      <p>p2</p>
   </div>
</body>
</html>
```

- 块元素的宽可继承自父级，高度由内容决定

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
</head>
<body>
   <div style="width: 200px;height: 200px;border: 1px solid black;">div1
      <div>div2
         <p>p1</p>
      </div>
      <p>p2</p>
   </div>
</body>
</html>
```



# 12.字体样式

```css
div{
    font-size: 16px;
    font-weight: bold;
    font-style: normal;
    font-family: cursive;
    color: red;
}
```

字符间距：`letter-spacing: 10px;`

对齐方式：`text-align: left center right justify;`

大小写字母：`text-transform: uppercase lowercase capitalize;`

文字装饰：`text-decoration: none line-through overline underline;`

---

```
r红色    g绿色    b蓝色
00-ff   00-ff   00-ff
```

```css
color: rgb(255, 255, 255);
color: #ff0000; 
color: #f00;
color: red;
```

```css
background-color: red;
background-image: url("logo.png");
background-repeat: no-repeat/repeat-x/repeat-y;
background-position: 100px 100px;

background: color image repeat position;
```



# 13.有序列表

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
</head>
<body>
   <ol type="A" reversed="reversed" start="4">
      <li></li>
      <li></li>
      <li></li>
   </ol>
</body>
</html>
```



# 14.无序列表

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
</head>
<body>
   <ul type="">
      <li>1</li>
      <li>2</li>
      <li>3</li>
   </ul>
</body>
</html>
```

```css
ul{
    list-style-type: ???;
}

none: 不使用项目符号
disc: 实心圆
circle: 空心圆
square: 实心方块
demical: 阿拉伯数字
lower-alpha: 小写英文字母
upper-alpha: 大写英文字母
lower-roman: 小写罗马数字
upper-roman: 大写罗马数字
```



# 15.自定义列表

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
</head>
<body>
   <dl>
      <dt>标题1</dt>
         <dd>内容1</dd>
      <dt>标题2</dt>
         <dd>内容2</dd>
   </dl>
</body>
</html>
```



# 16.`a`标签

- 超链接`<a href="http://www.baidu.com/">百度</a>`
- 打电话`<a href="tel: 17338459050">call me</a>`
- 发邮件`<a href="mailto: 1183476700@qq.com">email</a>`
- 黑客技术，协议限定符`<a href="javascript: while(1){alert('让你手欠')}">陷入循环</a>`
- 书签标记（锚点）：

```html
<a href="#end">最下面</a>
    br{$}*100
    <br>1
    <br>2
    <br id="mid3">3
    <br>4
    <br id="end">5
    <a href="#mid3">mid3</a>
<a href="#">最顶上</a>
```



# 17.图片映射

```html
<img src="./demo.jpg" usemap="#qiuqiumap" />地图

<map name="qiuiqumap">区域1
	<area shape="circle" coords="233,456,50" href="http://www.4399.com" target="-blank">
</map>

coords: 坐标点(中心点坐标 x233 y456,半径50) 
rect: 矩形,前两个坐标左上角,后两个坐标右下角
```



# 18.定位

`absolute` 绝对定位：脱离原来位置进行定位，相对于最近有定位的父级进行定位。若最近的父级无定位则相对于文档进行定位。

`relative` 相对定位：保留原来位置并且相对于原来位置进行定位。

`fixed` 固定定位。



# 19.盒模型

- 盒子内容 `content: width + height`
- 内边距 `padding`
- 盒子壁 `border`
- 外边距 `margin`



# 20.小练习

**固定容器始终位于屏幕正中间**

```css
div{
    position: absolute;
    top: 50%;
    left: 50%;
    width: wpx;
    height: hpx;
    margin-left: -w/2px;
    margin-top: -h/2px;
}
```

**margin塌陷，父级没有顶棚**

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <style>
      .wrapper{
         width: 100px;
         height: 100px;
         background-color: black;
         margin-left: 100px;
         margin-top: 100px;
      }
      .content{
         width: 50px;
         height: 50px;
         background-color: green;
         margin-left: 50px;
         margin-top: 200px; 
         /* 水平方向 margin 没问题 */
         /* 垂直方向 子元素margin-top的值必须设置的比父级大,否则不生效; */
         /* 当子元素的margin-top生效时,老子和儿子共用这个margin-top; */
         /* 这就是所谓的margin塌陷!!! */
      }
  </style>
</head>
<body>
   <body>
      <div class="wrapper">
         <div class="content"></div>
      </div>
  </body>
</body>
</html>
```

总结：垂直方向上的`margin`，父子元素是结合在一起取最大值的。

解决方式：`bcf(block format context)` 块级格式化上下文，改变盒子里的语法规则。触发盒子的`bfc`，只需要在子元素里添加样式，包括：

- `position: absolute;`
- `display: inline-block;`
- `float: left/right;` 



**margin合并**

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <style>
      .demo1{
         background-color: red;
         margin-bottom: 100px;
      }
      .demo2{
         background-color: green;
         margin-top: 100px; 
         /* 垂直方向上会导致较小的失效 */
      }
   </style>
</head>
<body>
   <body>
      <div class="demo1">a</div>
      <div class="demo2">b</div>
   </body>
</body>
</html>
```

**清浮动，注意：伪元素是行元素**

```css
.container::before, .conainer::after{
    display: table;
    content: "";
}
.container::after{
    clear: both;
}
```

**伪元素前后添加文本内容**

```html
<div>sxw</div>
<style>
    div::before{
        display: inline-block;
        width: 100px;
        height: 100px;
        border: 1px solid black;
        content: "打开前伪元素"
    }
    div::after{
        display: inline-block;
        width: 100px;
        height: 100px;
        border: 1px solid black;
        content: "打开后伪元素"
    }
</style>
```

**伪元素添加图片应用**

```html
<p>sxw</p>
<style>
    p::before{
        content: '哈哈';
        color: red;
    }
    p::after{
        display: inline-block;
        width: 100px;
        height: 100px;
        content: '';
        background: url('./a.jpg');
        background-size: cover;
    }
</style>
```

**远视图**

思路：实现子元素`width` + 父元素`padding` * 2 = 父元素`width` 即可。

```html
.wrapper{
    width: 50px;
    height: 50px;
    background-color: #000; 
    padding: 10px;
}
.box{
    width: 30px;
    height: 30px; 
    background-color: blue;
    padding: 10px;
}
.content{
    width: 10px;
    height: 10px; 
    padding: 10px; 
    background-color: green;
}
.contentSon{
    height: 10px;
    width: 10px;
    background-color: red; 
}
div.wrapper > div.box > div.content > div.contentSon
```

**画三角形**

```css
div{
    width: 0px;
    height: 0px; 
    border: 100px solid black; 
    border-left-color: red;
    border-top-color: transparent;
    border-right-color: transparent;
    border-bottom-color: transparent;
}
```

**淘宝导航栏**

```html
*{
    margin: 0;
    padding: 0;
    list-style: none;
    text-decoration: none;
}
.nav::before, .nav::after{ //设置父级,清除浮动
    display: table;
    content: "";
}
.container::after{
    clear: both;
}
.nav .list-item{
    float: left;
    margin: 0 10px;
    height: 30px;
    line-height: 30px; //垂直居中
}
.nav .list-item a{
    padding: 0 5px; 
    color: #f40; 
    display: inline-block;
    height: 30px; 
}
.nav .list-item a:hover{ 
    background-color: #f40;
    color: #fff;
    border-radius: 15px; 
}
<ul class="nav">
    <li class="list-item">
        <a href="#">天猫</a>
    </li>
    <li class="list-item">
        <a href="#">聚划算</a>
    </li>
</ul>
```

**溢出容器打点**

```css
p{
    width: 100px;
    height: 20px;
    border: 1px solid black;
    // 隐藏三件套:
    white-space: nowrap; 
    overflow: hidden; 
    text-overflow: ellipsis; 
}
```

**处理inline文本之间的空隙问题**

即：图片与图片之间的空隙。

```html
<img src="sxw.jpg">
<img src="sxw.jpg">
<img src="sxw.jpg">
<!-- 图片这样放一起在浏览器中显示中间会出现空隙 -->

<!-- 解决办法有两个: -->
	1.结构里这样写: <img src="sxw.jpg"><img src="sxw.jpg"><img src="sxw.jpg"> 常用方法!
	2.css里写成 
        img{
            width: 100px;
            height: 200px;
            margin-left: -6px; <!-- 设置这个maigin-left -->
        }
```
