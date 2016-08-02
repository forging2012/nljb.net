---
title: jQuery之AJAX的使用方法
date: '2016-05-29'
description:
categories:

tags:other

---

>

#### jQuery之AJAX的使用方法

>

    什么是 AJAX？
    AJAX = 异步 JavaScript 和 XML（Asynchronous JavaScript and XML）。
    简短地说，在不重载整个网页的情况下，AJAX 通过后台加载数据，并在网页上进行显示。
    使用 AJAX 的应用程序案例：谷歌地图、腾讯微博、优酷视频、人人网等等。

>

	通过 jQuery AJAX 方法，您能够使用 HTTP Get 和 HTTP Post
    从远程服务器上请求文本、HTML、XML 或 JSON
    同时您能够把这些外部数据直接载入网页的被选元素中。

>

---

>

#### jQuery load()

>

***load() 方法从服务器加载数据，并把返回的数据放入被选元素中。***

>

	$(selector).load(URL,data,callback);

* 必需的 URL 参数规定您希望加载的 URL。
* 可选的 data 参数规定与请求一同发送的查询字符串键/值对集合。
* 可选的 callback 参数是 load() 方法完成后所执行的函数名称。

>

    // 这是示例文件（"demo_test.txt"）的内容：
    <h2>jQuery and AJAX is FUN!!!</h2>
    <p id="p1">This is some text in a paragraph.</p>

>

	// 下面的例子会把文件 "demo_test.txt" 的内容加载到指定的 <div> 元素中：
    <script>
    $(document).ready(function(){
      $("#btn1").click(function(){
        $('#test').load('/example/jquery/demo_test.txt');
      })
    })
    </script>
    <h3 id="test">请点击下面的按钮，通过 jQuery AJAX 改变这段文本。</h3>
    <button id="btn1" type="button">获得外部的内容</button>

>

    // 也可以把 jQuery 选择器添加到 URL 参数。
    // 下面的例子把 "demo_test.txt" 文件中 id="p1" 的元素的内容，加载到指定的 <div> 元素中：
    $(document).ready(function(){
      $("button").click(function(){
        $("#div1").load("/example/jquery/demo_test.txt #p1");
      });
    });
    <div id="div1"><h2>使用 jQuery AJAX 来改变文本</h2></div>
    <button>获得外部内容</button>

>

    // 可选的 callback 参数规定当 load() 方法完成后所要允许的回调函数。回调函数可以设置不同的参数：
    // responseTxt - 包含调用成功时的结果内容
    // statusTXT - 包含调用的状态
    // xhr - 包含 XMLHttpRequest 对象
    $(document).ready(function(){
      $("button").click(function(){
        $("#div1").load("/example/jquery/demo_test.txt",function(responseTxt,statusTxt,xhr){
          if(statusTxt=="success")
            alert("外部内容加载成功！");
          if(statusTxt=="error")
            alert("Error: "+xhr.status+": "+xhr.statusText);
        });
      });
    });
    <div id="div1"><h2>使用 jQuery AJAX 来改变文本</h2></div>
    <button>获得外部内容</button>

>

---

>

#### HTTP 请求：GET vs. POST

>

***两种在客户端和服务器端进行请求-响应的常用方法是：GET 和 POST。***

* GET - 从指定的资源请求数据
* POST - 向指定的资源提交要处理的数据
* GET 基本上用于从服务器获得（取回）数据。注释：GET 方法可能返回缓存数据。
* POST 也可用于从服务器获取数据。不过，POST 方法不会缓存数据，并且常用于连同请求一起发送数据。

>

---

>

#### jQuery get()

>

***$.get() 方法通过 HTTP GET 请求从服务器上请求数据。***

	$.get(URL,callback);

* 必需的 URL 参数规定您希望请求的 URL。
* 可选的 callback 参数是请求成功后所执行的函数名。

>

	// 下面的例子使用 $.get() 方法从服务器上的一个文件中取回数据：
    // $.get() 的第一个参数是我们希望请求的 URL（"demo_test.asp"）。
	// 第二个参数是回调函数。第一个回调参数存有被请求页面的内容，第二个回调参数存有请求的状态。
    $(document).ready(function(){
      $("button").click(function(){
        $.get("/example/jquery/demo_test.asp",function(data,status){
          alert("数据：" + data + "\n状态：" + status);
        });
      });
    });
    <button>向页面发送 HTTP GET 请求，然后获得返回的结果</button>

>

---

>

#### jQuery $.post()

>

***$.post() 方法通过 HTTP POST 请求从服务器上请求数据。***

	$.post(URL,data,callback);

* 必需的 URL 参数规定您希望请求的 URL。
* 可选的 data 参数规定连同请求发送的数据。
* 可选的 callback 参数是请求成功后所执行的函数名。

>

	// 下面的例子使用 $.post() 连同请求一起发送数据：
	// $.post() 的第一个参数是我们希望请求的 URL ("demo_test_post.asp")。
	// 然后我们连同请求（name 和 city）一起发送数据。
	// "demo_test_post.asp" 中的 ASP 脚本读取这些参数，对它们进行处理，然后返回结果。
	// 第三个参数是回调函数。第一个回调参数存有被请求页面的内容，而第二个参数存有请求的状态。
    $(document).ready(function(){
      $("button").click(function(){
        $.post("/example/jquery/demo_test_post.asp",
        {
          name:"Donald Duck",
          city:"Duckburg"
        },
        function(data,status){
          alert("数据：" + data + "\n状态：" + status);
        });
      });
    });
    <button>向页面发送 HTTP POST 请求，并获得返回的结果</button>

>

---

>

#### jQuery $.getJSON()

>

	jQuery.getJSON(url,data,success(data,status,xhr))

>

* url	必需。规定将请求发送的哪个 URL。
* data	可选。规定连同请求发送到服务器的数据。
* success(data,status,xhr) 可选。规定当请求成功时运行的函数。

>

* success(data,status,xhr) 额外的参数：
* response - 包含来自请求的结果数据
* status - 包含请求的状态
* xhr - 包含 XMLHttpRequest 对象

>

	// 例子
    $(document).ready(function(){
      $("button").click(function(){
        $.getJSON("/example/jquery/demo_ajax_json.js",function(result){
          $.each(result, function(i, field){
            $("p").append(field + " ");
          });
        });
      });
    });
    <button>获得 JSON 数据</button>
    <p></p>

>

    // 从 Flickr JSONP API 载入 4 张最新的关于猫的图片：
    // HTML 代码：
    <div id="images"></div>
    // jQuery 代码：
    $.getJSON("http://api.flickr.com/services/feeds/photos_public.gne?
    tags=cat&tagmode=any&format=json&jsoncallback=?", function(data){
      $.each(data.items, function(i,item){
        $("<img/>").attr("src", item.media.m).appendTo("#images");
        if ( i == 3 ) return false;
      });
    });

>

    // 从 test.js 载入 JSON 数据，附加参数，显示 JSON 数据中一个 name 字段数据：
    $.getJSON("test.js", { name: "John", time: "2pm" }, function(json){
      alert("JSON Data: " + json.users[3].name);
    });

>

---

>

#### Evolver 项目所使用的 getJson()

>

    $(document).ready(function () {
        $("#scan_btn").click(function (e) {
            requestJSON("/api/get/report", {"mid": $("#scan_mid").val()});
        });
        var today = $("#today");
        var aweek = $("#aweek");
        var li_today = $("#li_today");
        var li_aweek = $("#li_aweek");
        today.click(function (e) {
            li_today.removeClass();
            li_aweek.removeClass();
            li_today.addClass("active");
            requestJSON("/api/get/report", {"date": "today"});
        });
        aweek.click(function (e) {
            li_today.removeClass();
            li_aweek.removeClass();
            li_aweek.addClass("active");
            requestJSON("/api/get/report", {"date": "aweek"});
        });
        today.click();
    });

    function requestJSON(addr, data) {
        $("#lines_tbody").html("");
        // 请求JSON数据
        $.getJSON(addr, data, function (json) {
            // 打印JSON数据
            console.log(JSON.stringify(json));
            if (json.ok) {
                var data = json.data;
                $("#stats_danger").text("异常" + "（" + data.stats.danger + "）");
                $("#stats_warning").text("警告" + "（" + data.stats.warning + "）");
                $("#stats_default").text("正常" + "（" + data.stats.default + "）");
                var lines = data.lines;
                $.each(lines, function (index, content) {
                    var class_tr = '<tr>';
                    var class_lv = "正常状态";
                    var class_tm;
                    if (content.level == 1) {
                        class_tr = '<tr class="warning">';
                        class_lv = "警告状态";
                    }
                    if (content.level == 2) {
                        class_tr = '<tr class="danger">';
                        class_lv = "异常状态";
                    }
                    if (content.timeout) {
                        class_tm = '<span class="label label-warning">超时</span>';
                    } else {
                        class_tm = '<span class="label label-success">有效</span>';
                    }
                    if (content.process.xplay == "") {
                        content.process.xplay = "error";
                    }
                    if (content.process.peanut == "") {
                        content.process.peanut = "error";
                    }
                    if (content.process.watchdog == "") {
                        content.process.watchdog = "error";
                    }
                    if (content.process.x == "") {
                        content.process.x = "error";
                    }
                    $("#lines_tbody").append(
                        class_tr +
                        "<td>" + content.id + "</td>" +
                        "<td>" + content.proxy + "</td>" +
                        '<td class="dropdown">' +
                        '<a class="dropdown-toggle" data-toggle="dropdown" href="#">' +
                        class_lv +
                        '<span class="caret"></span>' +
                        '</a>' +
                        '<ul class="dropdown-menu" aria-describedby="dropdownMenu1">' +
                        '<li class="text-center">' + "xplay(" + content.process.xplay + ')</li>' +
                        '<li class="text-center">' + "peanut(" + content.process.peanut + ')</li>' +
                        '<li class="text-center">' + "watchdog(" + content.process.watchdog + ')</li>' +
                        '<li class="text-center">' + "x-window(" + content.process.x + ')</li>' +
                        '</ul>' +
                        "</td>" +
                        "<td>" + content.pile + "</td>" +
                        "<td>" + content.uptime + "(H)" + "</td>" +
                        "<td>" + content.evolver_sleep + "(S)"+"</td>" +
                        "<td>" + content.term_time + "</td>" +
                        "<td>" + content.create_time + "</td>" +
                        "<td>" + class_tm + "</td>" +
                        "</tr>"
                    );
                });
            } else {
                console.log(json.data);
            }
        });
    }

>
