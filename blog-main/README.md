# 新生项目课程：云计算环境下的博客系统开发实践
## 开发环境：
1. 操作系统：WIndows11
2. 编程工具：Visual Studio
3. 浏览器：Edge浏览器
## 项目描述：
本项目使用 Node.js 作为后端运行环境，Express 框架来处理 HTTP 请求，以及 MongoDB 作为数据库存储文章及用户数据，实现了用户的注册和登录功能，以及用户对文章的增，删，改，查等基础功能

### 代码具体讲解：

### 1.登陆页面
```
<!DOCTYPE html>
<html>
<head>
  <title>Blog Login</title>
  <link rel="stylesheet" type="text/css" href="/css/bootstrap.min.css" />
  <% if (message) { %>
     <script>alert('<%= message %>');</script>
  <% } %>//若查不到登录信息则显示警告信息
  <style>
  body{
    background: url("/image/background.png") no-repeat fixed center;
    color: white;
  }
  .card-body{
    background-color: rgba(0, 0, 0, 0.6);
  }
  </style>
</head>

<body>
<div class="container">
<div class="card mt-4 bg-transparent">
    <div class="card-body">
    <form action="/login" method="POST">
      <h1 >Login</h1>
      <div class="form-group">
      <label for="username">username</label>
      <textarea name="username" id="username" class="form-control bg-transparent text-light"></textarea>//用户名输入
      </div>

      <div class="form-group">
      <label for="password">password</label>
      <textarea name="password" id="password" class="form-control bg-transparent text-light"></textarea>//密码输入
      </div>

      <a href="/" class="btn btn-secondary">Cancel</a>//取消按钮
      <button type="submit" class="btn btn-primary">Login</button>
      <a href="/register" class="btn btn-info">Register</a>//注册页面链接
    </form>
    </div>
</div>
</div>

</body>
</html>
```
### 2.注册页面
```
<!DOCTYPE html>
<html>
<head>
  <title>Register</title>
  <link rel="stylesheet" type="text/css" href="/css/bootstrap.min.css" />
  <style>
  body{
    background: url("/image/background.png") no-repeat fixed center;
    color: white;
  }
  .card-body{
    background-color: rgba(0, 0, 0, 0.6);
  }
  </style>
</head>
<body>
<div class="container">
<div class="card mt-4 bg-transparent">
    <div class="card-body">
    <form action="/register" method="POST">
      <h1 >Register</h1>
      <div class="form-group">
      <label for="username">new username</label>
      <textarea name="username" id="username" class="form-control bg-transparent text-light"></textarea>//用户名输入
      </div>

      <div class="form-group">
      <label for="password">new password</label>
      <textarea name="password" id="password" class="form-control bg-transparent text-light"></textarea>//密码输入
      </div>

      <a href="/" class="btn btn-secondary">Cancel</a>//返回初始页面
      <button type="submit" class="btn btn-primary">Save</button>//提交
    </form>
    </div>
</div>
<div>

</body>
</html>
```
### 3.文章索引页面
```
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Blog</title>
  <link rel="stylesheet" type="text/css" href="/css/bootstrap.min.css" />
  <style>
  .left {float: left;}
  .right {float: right;}
  .clear {clear: both;}//设置浮动样式
  body{
    background: url("/image/background.png") no-repeat fixed center;//设置背景图片
    color: white;
  }
  .card-body{
    background-color: rgba(0, 0, 0, 0.6);
  }
  </style>
</head>
<body>
  <div class="container">
    <h1 class="mt-4 left">Articles</h1>
    <h1 class="mt-4 right"><%= name %></h1>//显示用户名（文章作者）
    <div class="clear">
    <hr/>
    <a href="/new" class="btn btn-success">New Article</a>
    </div>
    <% articles.forEach(article => { %>//检索并渲染文章
    <div class="card mt-4 bg-transparent">
        <div class="card-body">
        <h2><%= article.title %></h2>//标题
            <div class="text-muted mb-2">
               <%= article.createdAt.toLocaleDateString() %>//日期
            </div>
        <p><%= article.description %></p>//文章信息和简介
        <a href="/display/<%= article._id %>" class="btn btn-primary">Read More</a>//展开内容链接
        <a href="/edit/<%= article._id %>" class="btn btn-info">Edit</a>//编辑链接
        <form action="/<%= article._id %>?_method=DELETE" method="POST" class="d-inline">
            <button type="submit" class="btn btn-danger">Delete</button>//删除链接
        </form>
        </div>
    </div>
    <% }) %>
  </div>
</body>
</html>
```
### 4.文章内容显示页面
```
<!DOCTYPE html>
<html>
<head>
  <title>Blog Display</title>
  <link rel="stylesheet" type="text/css" href="/css/bootstrap.min.css" />
  <style>
  body{
    background: url("/image/background.png") no-repeat fixed center;
    color: white;
  }
  .card-body{
    background-color: rgba(0, 0, 0, 0.6);
  }
  </style>
</head>
<body>
  <div class="container">
    <div class="card mt-4 bg-transparent">
      <div class="card-body">

      <h1><%= article.title %></h1>//文章标题
      <div class="text-muted mb-2"><%= article.createdAt.toLocaleDateString() %></div>//日期
      <hr/>
      <p><%= article.description %></p>//描述
      <hr/>
      <p><%- article.html %></p>//正文
      <a href="/" class="btn btn-secondary">HOME</a>//返回初始页面
      <a href="/edit/<%= article._id %>" class="btn btn-info">Edit</a>//编辑链接
      </div>
    </div>
  </div>
</body>
</html>
```
### 5.新建文章页面
```
<html>
<head>
  <title>Blog New</title>
  <link rel="stylesheet" type="text/css" href="/css/bootstrap.min.css" />
  <style>
  body{
    background: url("/image/background.png") no-repeat fixed center;
    color: white;
  }
  .card-body{
    background-color: rgba(0, 0, 0, 0.6);
  }
  </style>
</head>

<body>
<div class="container">
<h1>New Article</h1>
<form action="/new" method="POST">
<div class="card mt-4 bg-transparent">
    <div class="card-body">
    <div class="form-group">
    <label for="title">Title</label>
    <input required type="text" name="title" id="title" class="form-control bg-transparent text-light">//标题输入
    </div>

    <div class="form-group">
    <label for="description">Description</label>
    <textarea name="description" id="description" class="form-control bg-transparent text-light"></textarea>//描述输入
    </div>

    <div class="form-group">
    <label for="content">Content</label>
    <textarea name="content" id="content" class="form-control bg-transparent text-light"></textarea>//内容输入
    </div>

    <a href="/" class="btn btn-secondary">Cancel</a>//返回初始页面
    <button type="submit" class="btn btn-primary">Save</button>//保存
    </div>
</div>
</form>
</div>
</body>

</html>
```
### 6.编辑文章页面
```
<html>
<head>
  <title>Blog Edit</title>
  <link rel="stylesheet" type="text/css" href="/css/bootstrap.min.css" />
  <style>
  body{
    background: url("/image/background.png") no-repeat fixed center;//背景图片设置
    color: white;
  }
  .card-body{
    background-color: rgba(0, 0, 0, 0.6);
  }
  </style>
</head>

<body>
<div class="container">
<h1>Edit Article</h1>
<form action="/<%= article._id %>?_method=PUT" method="POST">
<div class="card mt-4 bg-transparent">
    <div class="card-body">
    <div class="form-group">
    <label for="title">Title</label>
    <input required value="<%= article.title %>" type="text" name="title" id="title" class="form-control bg-transparent text-light">//标题输入框
    </div>

    <div class="form-group">
    <label for="description">Description</label>
    <textarea name="description" id="description" class="form-control bg-transparent text-light"><%= article.description %></textarea>//文章简介输入框
    </div>

    <div class="form-group">
    <label for="content">Content</label>
    <textarea name="content" id="content" class="form-control bg-transparent text-light"><%= article.content %></textarea>//文章内容输入框
    </div>

    <a href="/" class="btn btn-secondary">Cancel</a>//取消
    <button type="submit" class="btn btn-primary">Save</button>//保存
    </div>
</div>    
</form>
</div>
</body>
</html>
```

## 项目总结
该项目基本实现了一个简易博客系统的功能，通过完成这个项目，我学会了基本的JavaScript和html语法，深入理解了网页前后端的运行逻辑。

## 工作量统计表
| 统计 | 基础功能 | 新增功能1 | 新增功能2 | 新增功能3 |
| :--------: | :--------: | :--------: | :--------: | :--------: | :--------: | :--------: |
| 描述 | 博客系统增删改查功能 | 制作登录和注册页面 | 实现readmore功能和时间作者显示 | 引用bootstrap框架进行界面美化 |
| 学时 | 6 | 2 | 1 | 2 | 
