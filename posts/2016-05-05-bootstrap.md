---
title: bootstrap
date: '2016-05-05'
description:
categories:

tags:other

---

>

##### Bootstrap

>

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <!-- 设置viewport -->
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title>NLJB</title>
        <!-- 引入框架 -->
        <script src="js/jquery.js"></script>
        <script src="js/bootstrap.js"></script>
        <!-- 引入样式 -->
        <link rel="stylesheet" href="css/bootstrap.css">
        <link rel="stylesheet" href="//cdn.bootcss.com/bootstrap/3.3.5/css/bootstrap.min.css">
        <!-- 自定义样式 -->
        <style>
            body {
                padding-top: 50px;
            }

            .start {
                padding: 40px 15px;
                text-align: center;
            }
        </style>
    </head>
    <body>
    <!-- 导航条 -->
    <!-- 样式： avbar-invers 和 navbar-default -->
    <!-- 位置:  navbar-fixed-top 和 navbar-fixed-bottom -->
    <nav class="navbar navbar-inverse navbar-fixed-top">
        <div class="container">
            <!-- 导航头部 navbar-header -->
            <div class="navbar-header">
                <a href="#" class="navbar-brand">NLJB</a>
            </div>
            <!-- 导航项目 -->
            <!-- 选中状态 active -->
            <div id="navbar" class="collapse navbar-collapse">
                <ul class="nav navbar-nav" id="mynav">
                    <li class="active"><a href="#">Home</a></li>
                    <li><a href="#">About</a></li>
                </ul>
                <!-- 添加表单 -->
                <form class="navbar-form navbar-left">
                    <div class="form-group">
                        <input type="text" class="form-control" placeholder="搜索">
                    </div>
                    <button type="submit" class="btn btn-default">搜索</button>
                </form>
                <!- 右对齐 -->
                <ul class="nav navbar-nav navbar-right">
                    <li class="dropdown">
                        <a href="#" class="dropdown-toggle" data-toggle="dropdown">退出
                            <span class="caret"></span>
                        </a>
                        <ul class="dropdown-menu">
                            <li><a href="#">关于</a></li>
                        </ul>
                    </li>
                </ul>
                <ul class="nav navbar-btn navbar-right ">
                    <!-- 下拉菜单与按钮组合 -->
                    <div class="btn-group">
                        <button class="btn btn-info dropdown-toggle" type="button" data-toggle="dropdown">
                            菜单
                            <!-- 向下箭头 -->
                            <span class="caret"></span>
                        </button>
                        <ul class="dropdown-menu" aria-describedby="dropdownMenu1">
                            <li><a href="#">一</a></li>
                            <li><a href="#">二</a></li>
                        </ul>
                    </div>
                </ul>
            </div>
        </div>
    </nav>
    <script>
        $("#mynav a").click(function (e) {
            e.preventDefault()
            $(this).tab("show")
        })
    </script>
    <!-- 页头 -->
    <div class="page-header">
        <div class="container">
            <p>NLJB</p>
        </div>
    </div>
    <!-- 巨幕 -->
    <div class="container">
        <div class="jumbotron">
            <h1>欢迎来到NLJB</h1>
            <p>欢迎来到这个巨幕展示页面 .....</p>
            <p><a class="btn btn-success">详情</a></p>
        </div>
    </div>
    <!-- 路径导航 -->
    <nav class="navbar navbar-default navbar-fixed-bottom">
        <div class="container">
            <ol class="breadcrumb">
                <li><a href="#">Home</a></li>
                <li><a href="#">Lib</a></li>
                <li><a href="#">Data</a></li>
            </ol>
        </div>
    </nav>
    <div class="container">
        <h1>NLJB(h1)</h1>
        <h2>NLJB(h2)</h2>
        <h3>NLJB(h3)</h3>
        <h4>NLJB(h4)</h4>
        <!-- 小号字体 -->
        <p>
            <small>NLJB(small)</small>
        </p>
        <!-- 标记 -->
        <p>
            <mark>NLJB(mark)</mark>
        </p>
        <!-- 删除 -->
        <p>
            <del>NLJB(del)</del>
        </p>
        <!-- 缩略语 -->
        <p>
            <abbr title="attribute">NLJB</abbr>
        </p>
        <!-- 内联代码 -->
        <p>
            <code>for{ print("NLJB") }</code>
        </p>
        <!-- 输入显示 -->
        <p>
            <kbd>cmd</kbd>
        </p>
        <!-- 输入显示 -->
        <pre>
        for {
            print(NLJB)
        }
        </pre>
        <!-- 变量输出 -->
        <p>
            <var>x</var> = <var>a</var> + <var>b</var>
        </p>
        <!-- 输出显示 -->
        <p>
            <samp># ifconfig</samp>
        </p>
        <!-- 突出显示 -->
        <p class="lead">NLJB(lead)</p>
        <!-- 相关 -->
        <p class="text-left">NLJB(text-left)</p>
        <p class="text-right">NLJB(text-right)</p>
        <p class="text-center">NLJB(text-center)</p>
        <p class="text-lowercase">NLJB(text-lowercase)</p>
        <p class="text-uppercase">nljb(text-uppercase)</p>
        <p class="text-capitalize">nljb(text-capitalize)</p>
        <!-- 无样式列表 -->
        <ul class="list-unstyled">
            <li>1</li>
            <li>2</li>
            <li>3</li>
        </ul>
    </div>
    <!-- 容器class必须为container -->
    <!-- 容器 container-fluid（百分百最宽） -->
    <div class="container">
        <div class="start">
            <!-- 行（最多包含12列） -->
            <div class="row">
                <!-- 1 代表 1/12 -->
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
            </div>
            <!--  行 -->
            <div class="row">
                <div class="col-md-2">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
            </div>
            <!--  行 -->
            <div class="row">
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
                <div class="col-md-1">col_md-1</div>
            </div>
            <!-- 偏移量 -->
            <div class="row">
                <div class="col-md-1 col-lg-offset-2">col_md-1</div>
                <div class="col-md-1 col-lg-offset-4">col_md-1</div>
                <div class="col-md-1 col-lg-offset-3">col_md-1</div>
            </div>
            <!-- 嵌套 -->
            <div class="row" style="background-color: #5e5e5e">
                <div class="col-md-9" style="background-color: #4cae4c">
                    One
                    <div class="row" style="background-color: #5bc0de">
                        <div class="col-md-2">A</div>
                        <div class="col-md-2">B</div>
                    </div>
                </div>
            </div>
            <!-- 排序：从左到右用push  从右往左用pull -->
            <div class="row">
                <div class="col-md-9 col-md-push-3" style="background-color: #5e5e5e">col_md-9</div>
                <div class="col-md-3 col-md-pull-9" style="background-color: #5bc0de">col_md-3</div>
            </div>
        </div>
    </div>
    <!-- 表格 -->
    <div class="container">
        <!-- table-striped 斑马线样式 -->
        <!-- table-bordered 边框 -->
        <!-- table-hover 鼠标悬停 -->
        <!-- table-condensed 紧凑型 -->
        <table class="table table-striped table-bordered table-hover table-condensed">
            <!-- 选中样式 -->
            <thead>
            <tr>
                <th>标题</th>
                <th>标题</th>
                <th>标题</th>
            </tr>
            </thead>
            <tbody>
            <tr>
                <td>内容</td>
                <td>内容</td>
                <td>内容</td>
            </tr>
            <tr class="active">
                <td>内容</td>
                <td>内容</td>
                <td>内容</td>
            </tr>
            <tr class="success">
                <td>内容</td>
                <td>内容</td>
                <td>内容</td>
            </tr>
            <tr class="info">
                <td>内容</td>
                <td>内容</td>
                <td>内容</td>
            </tr>
            <tr class="warning">
                <td>内容</td>
                <td>内容</td>
                <td>内容</td>
            </tr>
            <tr class="danger">
                <td>内容</td>
                <td>内容</td>
                <td>内容</td>
            </tr>
            </tbody>
        </table>
    </div>
    <div class="container">
        <!-- 表单 -->
        <form role="form">
            <!-- form-group 必须在 form 中　-->
            <div class="form-group">
                <!-- 这个 label 必须存在 -->
                <!-- 可以通过 sr-only 隐藏 -->
                <label class="sr-only">隐 藏：</label>
                <input type="text" class="form-control" placeholder="Text">
            </div>
            <div class="form-group">
                <p><label>文 件：</label><input type="file"></p>
                <!-- 辅助文本 help-block -->
                <p class="help-block">选择需要上传的文件</p>
            </div>
        </form>
        <!-- 内联表单 form-control -->
        <!-- 内联表单的宽度是自适应的 -->
        <form class="form-inline">
            <div class="form-group">
                <!-- type 可以指定输入的类型，比如 date email 等 -->
                <label>用 户：</label>
                <input type="email" class="form-control" placeholder="Email">
            </div>
            <div class="form-group">
                <label>密 码：</label>
                <input type="password" class="form-control" placeholder="Password">
            </div>
        </form>
    </div>
    <div class="container" style="padding-top: 10px">
        <form>
            <div class="form-group">
                <label for="name">内 容：</label>
                <textarea class="form-control" id="name">Hello</textarea>
            </div>
        </form>
    </div>
    <div class="container">
        <!-- 水平表单 form-horizontal -->
        <!-- 使用栅格系统实现水平对齐 -->
        <form class="form-horizontal">
            <div class="form-group">
                <label class="col-md-1 control-label">用 户：</label>
                <div class="col-md-4">
                    <input type="email" class="form-control" placeholder="Email">
                </div>
            </div>
            <div class="form-group">
                <label class="col-md-1 control-label">密 码：</label>
                <div class="col-md-4">
                    <input type="password" class="form-control" placeholder="Password">
                </div>
            </div>
            <div class="form-group">
                <div class="col-md-4 col-md-offset-1">
                    <div class="checkbox">
                        <label>
                            <input type="checkbox">记住密码
                        </label>
                    </div>
                </div>
            </div>
            <div class="form-group">
                <div class="row">
                    <div class="col-md-1 col-md-offset-2">
                        <button type="submit" class="btn btn-default">登录</button>
                    </div>
                    <div class="col-md-1">
                        <button type="reset" class="btn btn-default">取消</button>
                    </div>
                </div>
            </div>
        </form>
    </div>
    <div class="container">
        <form>
            <!-- 复选按钮 -->
            <div class="checkbox">
                <label>
                    <input type="checkbox" value="">时间
                </label>
            </div>
            <div class="checkbox">
                <label>
                    <input type="checkbox" value="">金钱
                </label>
            </div>
            <!-- 单选按钮 -->
            <!-- name 必须相同 -->
            <div class="radio">
                <label>
                    <input name="radio" type="radio" checked>时间
                </label>
                <label>
                    <input name="radio" type="radio">金钱
                </label>
            </div>
            <!-- 下拉选项 -->
            <div class="col-md-3">
                <label for="select_one" class="sr-only">选 项：</label>
                <select id="select_one" class="form-control">
                    <option>时间</option>
                    <option>金钱</option>
                </select>
            </div>
            <!-- 全部显示 multiple -->
            <div class="col-md-3">
                <label for="select_two" class="sr-only">菜 单：</label>
                <select multiple id="select_two" class="form-control">
                    <option>时间</option>
                    <option>金钱</option>
                </select>
            </div>
        </form>
    </div>
    <!-- 静态控件 form-control-static -->
    <!-- 纯文本和 label 元素放置于同一行 -->
    <div class="container">
        <form class="form-horizontal">
            <div class="form-group">
                <label class="col-md-1 control-label">Email：</label>
                <div class="col-md-11">
                    <p class="form-control-static">email@example.com</p>
                </div>
            </div>
        </form>
    </div>
    <!-- 禁用 disabled -->
    <!-- 只读 readonly -->
    <div class="container">
        <form>
            <div class="form-group">
                <input type="text" class="form-control" placeholder="disabled" disabled>
            </div>
            <div class="form-group">
                <input type="text" class="form-control" placeholder="readonly" readonly>
            </div>
        </form>
    </div>
    <!-- 全部禁用 fieldset disabled -->
    <div class="container">
        <form>
            <fieldset disabled>
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="fieldset">
                </div>
            </fieldset>
        </form>
    </div>
    <!-- 状态 -->
    <!-- 图标 span -->
    <!-- http://v3.bootcss.com/components/ -->
    <!-- <link rel="stylesheet" href="//cdn.bootcss.com/bootstrap/3.3.5/css/bootstrap.min.css"> -->
    <div class="container">
        <form>
            <div class="form-group has-warning has-feedback">
                <input type="text" class="form-control" placeholder="has-warning">
                <span class="glyphicon glyphicon-exclamation-sign form-control-feedback"></span>
            </div>
            <div class="form-group has-success has-feedback">
                <input type="text" class="form-control" placeholder="has-success">
                <span class="glyphicon glyphicon-ok form-control-feedback"></span>
            </div>
            <div class="form-group has-error has-feedback">
                <input type="text" class="form-control" placeholder="has-error">
                <span class="glyphicon glyphicon-remove-circle form-control-feedback"></span>
            </div>
        </form>
    </div>
    <!-- 按钮样式 -->
    <div class="container">
        <button type="button" class="btn btn-default">default</button>
        <button type="button" class="btn btn-info">info</button>
        <button type="button" class="btn btn-primary">primary</button>
        <button type="button" class="btn btn-success">success</button>
        <button type="button" class="btn btn-warning">warning</button>
        <button type="button" class="btn btn-link">link</button>
        <button type="button" class="btn btn-danger">danger</button>
    </div>
    <!-- 按钮大小 btn-lg btn-sm btn-xs-->
    <div class="container">
        <button type="button" class="btn btn-info btn-lg">info</button>
        <button type="button" class="btn btn-info">info</button>
        <button type="button" class="btn btn-info btn-sm">info</button>
        <button type="button" class="btn btn-info btn-xs">info</button>
    </div>
    <!-- 不同类型的按钮 -->
    <div class="container">
        <!-- 通过 disabled="disabled" 禁用 -->
        <button type="button" class="btn btn-primary" disabled="disabled">primary</button>
        <!-- 充满容器 btn-block -->
        <a href="#" class="btn btn-warning btn-block" disabled="disabled">warning </a>
        <input class="btn btn-success" type="button" value="input">
    </div>
    <div class="container">
        <img src="http://static.bootcss.com/www/assets/img/codeguide.png" alt="" class="img-circle">
        <img src="http://static.bootcss.com/www/assets/img/codeguide.png" alt="" class="img-rounded">
        <img src="http://static.bootcss.com/www/assets/img/codeguide.png" alt="" class="img-responsive">
        <img src="http://static.bootcss.com/www/assets/img/codeguide.png" alt="" class="img-thumbnail">
    </div>
    <!-- 下拉菜单-->
    <div class="container">
        <!-- 右侧 pull-right -->
        <!-- 左侧 pull-left -->
        <div class="dropdown pull-right">
            <button class="btn btn-default dropdown-toggle" type="button" data-toggle="dropdown">
                下拉菜单
                <!-- 向下箭头 -->
                <span class="caret"></span>
            </button>
            <!-- 右侧 dropdown-menu-right -->
            <ul class="dropdown-menu dropdown-menu-right" aria-describedby="dropdownMenu1">
                <!-- 标头 -->
                <li class="dropdown-header">大写</li>
                <li><a href="#">一</a></li>
                <li><a href="#">二</a></li>
                <!-- 不可用 -->
                <li class="disabled"><a href="#">三</a></li>
                <li><a href="#">四</a></li>
                <!-- 标头 -->
                <li class="dropdown-header">小写</li>
                <li><a href="#">1</a></li>
                <li><a href="#">2</a></li>
                <!-- 不可用 -->
                <li class="disabled"><a href="#">3</a></li>
                <li><a href="#">4</a></li>
                <!-- 分割线 -->
                <li class="divider"></li>
                <li><a href="#">结束</a></li>
            </ul>
        </div>
    </div>
    <!-- 按钮组 -->
    <div class="container">
        <!-- 垂直 -->
        <div class="btn-group-vertical">
            <button type="button" class="btn btn-success">左</button>
            <button type="button" class="btn btn-success">中</button>
            <button type="button" class="btn btn-success">右</button>
        </div>
        <!-- 默认水平 -->
        <div class="btn-group">
            <button type="button" class="btn btn-warning">左</button>
            <button type="button" class="btn btn-warning">中</button>
            <button type="button" class="btn btn-warning">右</button>
            <!-- 下拉菜单与按钮组合 -->
            <div class="btn-group">
                <button class="btn btn-info dropdown-toggle" type="button" data-toggle="dropdown">
                    下拉菜单
                    <!-- 向下箭头 -->
                    <span class="caret"></span>
                </button>
                <ul class="dropdown-menu" aria-describedby="dropdownMenu1">
                    <li><a href="#">一</a></li>
                    <li><a href="#">二</a></li>
                </ul>
            </div>
        </div>
        <!-- 平局充满 -->
        <div class="btn-group btn-group-justified">
            <div class="btn-group">
                <button type="button" class="btn btn-danger">左</button>
            </div>
            <div class="btn-group">
                <button type="button" class="btn btn-danger">中</button>
            </div>
            <div class="btn-group">
                <button type="button" class="btn btn-danger">右</button>
            </div>
        </div>
        <!-- 按钮工具栏 -->
        <div class="btn-toolbar">
            <!-- 按钮组的大小 btn-group-lg btn-group-sm btn-group-xs -->
            <div class="btn-group btn-group-lg">
                <button type="button" class="btn btn-default"><span class="glyphicon glyphicon-trash"></span></button>
                <button type="button" class="btn btn-default"><span class="glyphicon glyphicon-repeat"></span></button>
            </div>
            <div class="btn-group">
                <button type="button" class="btn btn-default"><span class="glyphicon glyphicon-align-right"></span></button>
            </div>
            <div class="btn-group btn-group-sm">
                <button type="button" class="btn btn-default"><span class="glyphicon glyphicon-volume-up"></span></button>
                <button type="button" class="btn btn-default"><span class="glyphicon glyphicon-font"></span></button>
                <button type="button" class="btn btn-default"><span class="glyphicon glyphicon-plus-sign"></span></button>
            </div>
            <div class="btn-group btn-group-xs">
                <button type="button" class="btn btn-default"><span class="glyphicon glyphicon-align-right"></span></button>
            </div>
        </div>
    </div>
    <!-- 分列式下拉菜单 + 按钮组 -->
    <div class="container">
        <!-- 按钮组 -->
        <!-- 向上箭头 dropup -->
        <div class="btn-group dropup">
            <button class="btn btn-default" type="button">下拉菜单</button>
            <button class="btn btn-info dropdown-toggle" type="button" data-toggle="dropdown">
                <!-- 向下箭头 -->
                <span class="caret"></span>
            </button>
            <!-- 右侧 dropdown-menu-right -->
            <ul class="dropdown-menu" aria-describedby="dropdownMenu1">
                <li><a href="#">一</a></li>
                <li><a href="#">二</a></li>
            </ul>
        </div>
    </div>
    <!-- 输入框组 -->
    <div class="container">
        <div class="input-group">
            <span class="input-group-addon">@</span>
            <input type="text" class="form-control" placeholder="UserNmae">
            <span class="input-group-addon">.00</span>
        </div>
        <!-- 尺寸可调节 -->
        <div class="input-group input-group-sm">
            <span class="input-group-addon">@</span>
            <input type="text" class="form-control" placeholder="UserNmae">
            <span class="input-group-addon">.00</span>
        </div>
    </div>
    <!-- 位置大小 -->
    <div class="container">
        <!-- 多选复选的输入框 -->
        <div class="row">
            <div class="col-md-3">
                <div class="input-group">
                      <span class="input-group-addon">
                        <label for="in"></label>
                        <input id="in" type="radio">
                    </span>
                    <input type="text" class="form-control" placeholder="UserNmae">
                </div>
            </div>
            <!-- 带按钮的输入框 -->
            <div class="col-md-3 col-lg-offset-1">
                <div class="input-group">
                      <span class="input-group-btn">
                          <button class="btn btn-default" type="button">@</button>
                    </span>
                    <input type="text" class="form-control" placeholder="UserNmae">
                </div>
            </div>
            <!-- 下拉菜单的输入框 -->
            <div class="col-md-3 col-lg-offset-1">
                <div class="input-group">
                    <div class="input-group-btn">
                        <button class="btn btn-info dropdown-toggle" type="button" data-toggle="dropdown">
                            下拉菜单
                            <!-- 向下箭头 -->
                            <span class="caret"></span>
                        </button>
                        <ul class="dropdown-menu" aria-describedby="dropdownMenu1">
                            <li><a href="#">一</a></li>
                            <li><a href="#">二</a></li>
                        </ul>
                    </div>
                    <input type="text" class="form-control" placeholder="UserNmae">
                </div>
            </div>
        </div>
    </div>
    <!-- 导航页 -->
    <!-- 样式 nav-tabs nav-pills -->
    <!-- 垂直显示 nav-stacked -->
    <!-- 充满全屏 nav-justified -->
    <div class="container">
        <ul id="mytab" class="nav nav-tabs nav-justified">
            <li class="active"><a href="#">新闻</a></li>
            <li class="dropdown">
                <a class="dropdown-toggle" data-toggle="dropdown" href="#">
                    影视
                    <span class="caret"></span>
                </a>
                <ul class="dropdown-menu" aria-describedby="dropdownMenu1">
                    <li><a href="#">一</a></li>
                    <li><a href="#">二</a></li>
                </ul>
            </li>
            <!-- 不可点击 -->
            <li class="disabled"><a href="#">体育</a></li>
        </ul>
    </div>
    <script>
        $("#mytab a").click(function (e) {
            e.preventDefault()
            $(this).tab("show")
        })
    </script>
    <!-- 媒体组件 -->
    <div class="container">
        <div class="media">
            <!-- 居左显示图像 -->
            <!-- 图像位置 media-bottom（居底） media-middle（居中） -->
            <a class="media-left media-middle">
                <img src="http://www.iconpng.com/png/gnome-desktop/applications-office.png">
            </a>
            <div class="media-body">
                <h5 class="media-heading">标题</h5>
                <p>内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</p>
                <!-- 媒体子项 -->
                <div class="media">
                    <a class="media-left">
                        <img src="http://www.iconpng.com/png/gnome-desktop/applications-office.png">
                    </a>
                    <div class="media-body">
                        <h5 class="media-heading">标题</h5>
                        <p>内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</p>
                    </div>
                </div>
            </div>
        </div>
        <div class="media">
            <div class="media-body">
                <h5 class="media-heading">标题</h5>
                <p>内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</p>
            </div>
            <!-- 居右显示图像 -->
            <a class="media-right">
                <img src="http://www.iconpng.com/png/gnome-desktop/applications-office.png">
            </a>
        </div>
    </div>
    <!-- 媒体组件列表 -->
    <div class="container">
        <ul class="media-list">
            <li class="media">
                <a class="media-left">
                    <img src="http://www.iconpng.com/png/gnome-desktop/applications-office.png">
                </a>
                <div class="media-body">
                    <h5 class="media-heading">标题</h5>
                    <p>内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</p>
                </div>
            </li>
        </ul>
    </div>
    <!-- 面板 -->
    <!-- 面板颜色 panel-info panel-warning panel-success -->
    <div class="container">
        <div class="panel panel-default panel-warning">
            <div class="panel-heading">
                标题
            </div>
            <!-- 盒子 -->
            <div class="panel-body">
                Hello World !!!<br>
                Hello World !!!<br>
                Hello World !!!<br>
                Hello World !!!<br>
            </div>
            <!-- 标注 -->
            <div class="panel-footer panel-info">
                www.nljb.net（标注）
            </div>
        </div>
    </div>
    <!-- 面板表格 -->
    <div class="container">
        <div class="panel panel-default panel-warning">
            <div class="panel-heading">
                标题
            </div>
            <!-- 盒子 -->
            <div class="panel-body">
                Hello World !!!<br>
                Hello World !!!<br>
                Hello World !!!<br>
                Hello World !!!<br>
            </div>
            <!-- 表格 -->
            <table class="table">
                <thead>
                <tr class="active">
                    <th>表格标题</th>
                    <th>表格标题</th>
                    <th>表格标题</th>
                    <th>表格标题</th>
                </tr>
                </thead>
                <tbody>
                <tr class="success">
                    <td>表格单元格</td>
                    <td>表格单元格</td>
                    <td>表格单元格</td>
                    <td>表格单元格</td>
                </tr>
                <tr class="success">
                    <td>表格单元格</td>
                    <td>表格单元格</td>
                    <td>表格单元格</td>
                    <td>表格单元格</td>
                </tr>
                <tr class="success">
                    <td>表格单元格</td>
                    <td>表格单元格</td>
                    <td>表格单元格</td>
                    <td>表格单元格</td>
                </tr>
                </tbody>
            </table>
            <!-- 标注 -->
            <div class="panel-footer panel-info">
                www.nljb.net
            </div>
        </div>
    </div>
    <style>
        .table th, .table td {
            text-align: center;
        }
    </style>
    <div class="container">
        <div class="panel panel-default">
            <div class="panel-heading">
                标题
            </div>
            <!-- 盒子 -->
            <div class="panel-body">
                Hello World !!!
            </div>
            <!-- 列表 -->
            <ul class="list-group">
                <li class="list-group-item">World !!!</li>
                <li class="list-group-item">World !!!</li>
                <li class="list-group-item">World !!!</li>
                <li class="list-group-item">World !!!</li>
            </ul>
        </div>
    </div>
    <!-- Well 组件 -->
    <!-- 内容凹陷效果 -->
    <div class="container">
        <!--<div class="embed-responsive embed-responsive-16by9">-->
        <!--<iframe class="embed-responsive-item" src="http://www.baidu.com"></iframe>-->
        <!--</div>-->
        <div class="well well-sm">
            Hello World （Well）!!!
        </div>
    </div>
    <!-- 分页 -->
    <div class="container">
        <nav>
            <ul class="pagination pagination-lg">
                <li class="disabled"><a href="#">&laquo;</a></li>
                <li><a href="#">1</a></li>
                <li><a href="#">2</a></li>
                <li><a href="#">3</a></li>
                <li><a href="#">4</a></li>
                <li class="active"><a href="#">&raquo;</a></li>
            </ul>
            <ul class="pagination">
                <li class="disabled"><a href="#">&laquo;</a></li>
                <li><a href="#">1</a></li>
                <li><a href="#">2</a></li>
                <li><a href="#">3</a></li>
                <li><a href="#">4</a></li>
                <li class="active"><a href="#">&raquo;</a></li>
            </ul>
            <ul class="pagination pagination-sm">
                <li class="disabled"><a href="#">&laquo;</a></li>
                <li><a href="#">1</a></li>
                <li><a href="#">2</a></li>
                <li><a href="#">3</a></li>
                <li><a href="#">4</a></li>
                <li class="active"><a href="#">&raquo;</a></li>
            </ul>
        </nav>
        <nav>
            <ul class="pager">
                <li><a href="#">下一页</a></li>
                <li><a href="#">上一页</a></li>
            </ul>
            <ul class="pager">
                <li class="previous"><a href="#">向前</a></li>
                <li class="next"><a href="#">向后</a></li>
            </ul>
        </nav>
    </div>
    <!-- 标签 -->
    <div class="container">
        <span class="label label-default">标签</span>
        <span class="label label-primary">标签</span>
        <span class="label label-info">标签</span>
        <span class="label label-warning">标签</span>
        <span class="label label-danger">标签</span>
        <span class="label label-success">标签</span>
    </div>
    <!-- 徽章 -->
    <div class="container">
        <a href="#">Message&nbsp;<span class="badge">30</span></a>
        <button type="button" class="btn btn-success">Message&nbsp;<span class="badge">30</span></button>
    </div>
    <!-- 缩略图 -->
    <div class="container">
        <div class="row">
            <div class="col-md-4">
                <div class="thumbnail">
                    <img src="http://www.iconpng.com/png/gnome-desktop/applications-office.png">
                    <div class="caption">
                        <h3>（缩略图）</h3>
                        <p>内容内容内容</p>
                        <p><a href="#" class="btn btn-primary">确定</a></p>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- 警告框 -->
    <div class="container">
        <div class="alert alert-success">
            Hello （警告框） !!!
        </div>
        <div class="alert alert-danger">
            Hello （警告框） !!!
        </div>
        <div class="alert alert-warning">
            Hello （警告框） !!!
        </div>
        <div class="alert alert-danger">
            Hello （警告框） !!!
        </div>
    </div>
    <!-- 可关闭的警告框 -->
    <div class="container">
        <div class="alert alert-success">
            Hello 大家好（警告框） !!!
            <a href="#" class="alert-link">（链接）</a>
            <button class="close" type="button" data-dismiss="alert">
                <span aria-disabled="true">&times;</span>
            </button>
        </div>
    </div>
    <!-- 进度条 -->
    <div class="container">
        <div class="progress">
            <!-- 条纹效果 progress-bar-striped -->
            <!-- 动画效果 active -->
            <div class="progress-bar progress-bar-striped active" aria-valuemin="10" aria-valuenow="50" aria-valuemax="100"
                 style="width: 60%">
                <span>60%</span>
            </div>
        </div>
        <div class="progress">
            <div class="progress-bar progress-bar-info" aria-valuemin="10" aria-valuenow="50" aria-valuemax="100"
                 style="width: 60%">
                <span>60%</span>
            </div>
        </div>
        <div class="progress">
            <div class="progress-bar progress-bar-danger" aria-valuemin="10" aria-valuenow="50" aria-valuemax="100"
                 style="width: 60%">
                <span>60%</span>
            </div>
        </div>
        <div class="progress">
            <div class="progress-bar progress-bar-success" aria-valuemin="10" aria-valuenow="50" aria-valuemax="100"
                 style="width: 60%">
                <span>60%</span>
            </div>
        </div>
        <div class="progress">
            <div class="progress-bar" style="width: 30%">
                <span>30%</span>
            </div>
            <div class="progress-bar progress-bar-warning progress-bar-striped" aria-valuemin="10" aria-valuenow="50"
                 aria-valuemax="100" style="width: 60%">
                <span>60%</span>
            </div>
        </div>
    </div>
    <!-- 列表组 -->
    <div class="container">
        <!-- 不可点击 -->
        <ul class="list-group">
            <!-- 徽章提示 -->
            <!-- 状态颜色 -->
            <li class="list-group-item list-group-item-info"><span class="badge">10</span>列表组</li>
            <li class="list-group-item list-group-item-warning">列表组</li>
            <li class="list-group-item list-group-item-danger">列表组</li>
            <li class="list-group-item list-group-item-success">列表组</li>
        </ul>
        <!-- 可以点击的 -->
        <div class="list-group">
            <!-- 比如禁用 disabled -->
            <!--比如选中 active -->
            <a href="#" class="list-group-item disabled">可点击列表组</a>
            <a href="#" class="list-group-item list-group-item-success active">可点击列表组</a>
        </div>
        <!-- 定制信息 -->
        <div class="list-group">
            <a href="#" class="list-group-item disabled">
                <h4 class="list-group-item-heading">列表组</h4>
                <p class="list-group-item-text">这里是内容 ...</p>
            </a>
            <a href="#" class="list-group-item list-group-item-success active">
                <h4 class="list-group-item-heading">列表组</h4>
                <p class="list-group-item-text">这里是内容 ...</p></a>
        </div>
    </div>
    <!-- 模态框 -->
    <div class="container">
        <!-- 过度效果：fade -->
        <div class="modal fade" id="modal" tabindex="-1">
            <!-- 大小：modal-sm modal-lg -->
            <div class="modal-dialog modal-lg">
                <div class="modal-content">
                    <div class="modal-header">
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                        <h4 class="modal-title">欢迎来到NLJB</h4>
                    </div>
                    <div class="modal-body">
                        Content
                        <form>
                            <div class="form-group">
                                <label for="myinput">Form</label>
                                <input type="text" class="form-control" id="myinput">
                            </div>
                        </form>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
                        <button type="button" class="btn btn-success" data-dismiss="modal">Save</button>
                    </div>
                </div>
            </div>
        </div>
        <button type="button" class="btn btn-success" data-toggle="modal" data-target="#modal" data-whatever="NLJB">模态框</button>
    </div>
    <script>
        $("#modal").on("show.bs.modal",function (event) {
            var button = $(event.relatedTarget)
            var rrr = button.data("whatever")
            var modal = $(this);
            modal.find(".modal-title").text(rrr)
            modal.find(".modal-body input").val(rrr)
        })
    </script>
    <!-- 地址信息 -->
    <div class="container" style="padding-top: 50px">
        <address class="pull-right">
            <strong>NLJB</strong><br>
            www.nljb.net<br>
            北京市海淀区上地
        </address>
    </div>
    </body>
    </html>

>

---

>
