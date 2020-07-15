##		art-template

###  1.基本使用

1. 下载 art-template 模板引擎库文件并在 HTML 页面中引入库文件

   ```html
   <script src="./js/template‐web.js"></script>
   ```

2. 创建模板   `<script type="text/html id="xxxx">` 注意：一定要指定id

   ```html
   <script id="tpl" type="text/html">
   	<div class="box"></div>
   </script>
   ```

3. 告诉模板引擎将哪一个模板和哪个数据进行拼接, 使用temple函数 `template(id, obj对象)`,并获取返回值 注意：这里一定是一个对象{}

   ```javascript
   var html = template('tpl', {username: 'zhangsan', age: '20'});
   ```

4. 通过模板语法告诉模板引擎，数据和html字符串要如何拼接

   ```html
   <script id="tpl" type="text/html">
   	<div class="box"> {{ username }} </div>
   </script>document.getElementById('container').innerHTML = html;
   ```

5. 将拼接好的html字符串添加到页面中

   ```javascript
   document.getElementById('container').innerHTML = html;
   ```



###  2.1模板语法：

###  	1.标准语法： {{ 数据 }}

		###  	2.原始语法：<%= 数据 %>

###  2.2输出

​	在某项数据中输出在模板中，标准语法和原始语法如下：

- 标准语法： {{ 数据 }}
- 原始语法： <%= 数据 %>

  ```html
<!-- 标准语法-->
<h2>{{value}}</h2>
<h2>{{a ? b : c}}</h2>
<h2>{{a + b}}</h2>
<!--原始语法-->
<h2><%= value %></h2>
<h2><%= a ? b : c %></h2>
<h2><%= a + b %></h2>
  ```

###  2.3原文输出

如果数据中携带HTML标签，默认模板引擎不会解析标签，会将其转义后输出。

- 标准语法： {{@ 数据}}
- 原始语法：<%- value %>

```html
<!--标准语法-->
<h2>{{@ value}}</h2>
<!--原始语法-->
<h2><%- value %></h2>
```

### 2.4条件判断

```html
<!--标准语法-->
{{if 条件}} ... {{/ if}}
{{if v1}} ... {{else if v2}}... {{/if}}
<!--原始语法-->
<% if (value) { %> 
    
<% } %>
<% if (v1) { %>
    
<% } else if (v2)  { %>
   
<% } else { %>
   
<% }%>
```

###  2.5循环

- 标准语法：{{ each 数据 }} {{/each}}
- 原始语法：<%for() { %> <% } %>

```html
<!-- 标准语法 -->
<script type="text/html">
    {{ each target}}
      {{$index}} {{$value}}
    {{/each}}
<!-- 原始语法 -->
    <% for (let i = 0; i < target.length; i++ { %>
       <%= i %> <%= target[i] %>
    <% } %>
</script>
<script>
  <script id="test" type="text/html">//遍历list里的src
	{{each list as item i}}
	<p> {{item.src}}</p>
	{{/each}}
</script>
<!-- 两种写法效果一致 -->
<script id="test" type="text/html">//遍历list里的src
{{each list }}
<p> {{$index}} --- {{$value.src}}</p>
{{/each}}
</script>
<!-- ++++++++++++++++++++++++++++++++++++++++++++++++++++++这里是模板遍历重点 -->
//尽量把json写成对象数组形式
<script>
var data={
  list:[
        {src:'aaa'},
        {src:'bbb'},
        {src:'ccc'},
        {src:'ddd'}
     ]
}
var html=template('test',data);
$("div").html(html);//插入div标签里
</script>
</html>
</script>
```

###   2.6子模版

使用子模版可以将网站公共区块(头部、底部)抽离到单独的文件中。

- 标准语法：{{include '模板'}}
- 原始语法： <% include('模板') %>

  ```html
  <script type="text/html">
  <!-- 标准语法 -->
  {{include './header.art'}}
  <!-- 原始语法 -->
   <% include('./header.art') %>
  </script>
  ```

###  2.7模板的继承和引入子模版的示例 (art-template)

```html
header.att文件
<!-- 头部 -->
<div class="header">
	<!-- 网站标志 -->
    <div class="logo fl">
      黑马程序员 <i>ITHEIMA</i>
    </div>
    <!-- /网站标志 -->
    <!-- 用户信息 -->
    <div class="info">
        <div class="profile dropdown fr">
            <span class="btn dropdown-toggle" data-toggle="dropdown">
				{{userInfo && userInfo.username}}
				<span class="caret"></span>
            </span>
            <ul class="dropdown-menu">
                <li><a href="user-edit.html">个人资料</a></li>
                <li><a href="/admin/logout">退出登录</a></li>
            </ul>
        </div>
    </div>
    <!-- /用户信息 -->
</div>
<!-- /头部 -->

```

```html
layout.art文件
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <title>Blog - Content Manager</title>
    <link rel="stylesheet" href="/admin/lib/bootstrap/css/bootstrap.min.css">
    <link rel="stylesheet" href="/admin/css/base.css">
    {{block 'link'}}{{/block}}
</head>

<body>
	{{block 'main'}} {{/block}}
	<script src="/admin/lib/jquery/dist/jquery.min.js"></script>
	<script src="/admin/lib/bootstrap/js/bootstrap.min.js"></script>
    <script src="/admin/js/common.js"></script>
	{{block 'script'}} {{/block}}
</body>

</html>
```

```html
article文件
<!-- 继承骨架文件 -->
{{extend './common/layout.art'}}

{{block 'main'}}

<!-- 引入子模版文件 -->

    {{include './common/header.art'}}
    <!-- 主体内容 -->
    <div class="content">
        {{include './common/aside.art'}}
        <div class="main">
            <!-- 分类标题 -->
            <div class="title">
                <h4>5b9a716cb2d2bf17706bcc0a</h4>
            </div>
            <!-- /分类标题 -->
            <form class="form-container">
                <div class="form-group">
                    <label>标题</label>
                    <input type="text" class="form-control" placeholder="请输入文章标题">
                </div>
                <div class="form-group">
                    <label>作者</label>
                    <input type="text" class="form-control" readonly>
                </div>
                <div class="form-group">
                    <label>发布时间</label>
                    <input type="date" class="form-control">
                </div>
                
                <div class="form-group">
                   <label for="exampleInputFile">文章封面</label>
                   <input type="file">
                   <div class="thumbnail-waper">
                       <img class="img-thumbnail" src="">
                   </div>
                </div>
                <div class="form-group">
                    <label>内容</label>
                    <textarea class="form-control" id="editor"></textarea>
                </div>
                <div class="buttons">
                    <input type="submit" class="btn btn-primary">
                </div>
            </form>
            
        </div>
    </div>
    <!-- /主体内容 -->
{{/block}}

{{block 'script'}}
    <script src="/admin/lib/ckeditor5/ckeditor.js"></script>
    <script type="text/javascript">
    
        let editor;

        ClassicEditor
                .create( document.querySelector('#editor'))
                .then(newEditor => {
                    editor = newEditor;
                })
                .catch( error => {
                    console.error( error );
                });

        // 获取数据
        // const editorData = editor.getData();
    </script>
{{/block}}
```

