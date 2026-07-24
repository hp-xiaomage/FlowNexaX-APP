# 💯 当前教程对应的是4.4.3及之后的版本💯

4.4.3版本开始，FlowNexaX支持自定义界面可以写代码的方式自定义展示的样式。

你需要关注以下几个方法

```
ckjl这四个字母代表的是调用FlowNexaX的接口。
//onJSError方法能实现把html文件中的错误发送到触控的运行日志中
ckjl.onJSError("传错误信息给FlowNexaX到运行日志中");

//getVar方法相当于获取变量的内容
ckjl.getVar("sys-字")

//setVar方法就是给变量设置内容
ckjl.setVar("sys-字", "这个内容是设置给变量的");
//setVar方法就是给变量设置内容，传入参数 true 代表变量内容要持久保持内容，支持系统变量
ckjl.setVar("sys-字", "这个内容是设置给变量的",true);
//setVar方法就是给变量设置内容，传入参数第一个 true 代表变量内容要持久保持，第二个 true 代表要存图片变量之前screenshot的位置，值支持系统变量
ckjl.setVar("sys-图", "图片变量地址",true,true);

//reqScreenshot方法代表你要给 sys-图 这个变量进行screenshot
ckjl.reqScreenshot("sys-图"); //不传screenshot默认代表screenshot
ckjl.reqScreenshot("sys-图","screenshot");
ckjl.reqScreenshot("sys-图","Take a screenshot and record the location");
ckjl.reqScreenshot("sys-图","Irregular screenshots");
ckjl.reqScreenshot("sys-图","Take irregular screenshots and record the location");


//reqScreenshotBack方法是搭配reqScreenshot一起用的，代表你给 sys-图 这个变量进行screenshot成功后会通过reqScreenshotBack来告诉你screenshot成功，并且把screenshot的图片链接url传给你，而vname就是你前面reqScreenshot("sys-图")这个sys-图 变量名字。
function reqScreenshotBack(vname,url){}

//reqSelPoint方法代表你要给 sys-坐单 这个变量进行屏幕选择坐标
ckjl.reqSelPoint("sys-坐单");
//reqSelPoint方法代表你要给 sys-坐多 这个坐标多个类型变量进行屏幕Frame selection of upper left and lower right coordinates,449版本开始支持
ckjl.reqSelPoint("sys-坐多","Frame selection of upper left and lower right coordinates");
//reqSelPoint方法代表你要给 sys-坐多 这个坐标多个类型变量进行屏幕选择多个坐标,449版本开始支持
ckjl.reqSelPoint("sys-坐多");

//reqSelPointBack方法是搭配reqSelPoint一起用的，代表你给 sys-坐单 这个变量进行屏幕选坐标成功后会通过reqSelPointBack来告诉你选取成功，并且把坐标内容pointContent传给你，而vname就是你前面reqSelPoint("sys-坐单")这个sys-坐单 变量名字。
function reqSelPointBack(vname,pointContent){}


//formCancel方法是自定义HTML界面配套的方法，点了界面的取消按钮就会调这个方法，而这里面的ckjl.formCancelBack()是标配的，你一定要调ckjl.formCancelBack()，否则弹框界面会不关闭
            async function formCancel() {
                document.getElementById("name").value = "点了取消";
                await sleep(3000);
                ckjl.formCancelBack();
            }


//formConfirm方法是自定义HTML界面配套的方法，点了界面的确定按钮就会调这个方法，而这里面的ckjl.formConfirmBack()是标配的，你一定要调ckjl.formConfirmBack()，否则弹框界面会不关闭
            async function formConfirm() {
                document.getElementById("name").value = "点了确认";
                await sleep(3000);
                ckjl.formConfirmBack();
            }


//formZoom方法代表缩小整个界面，455 版本开始支持
ckjl.formZoom();

//addSysVar方法，用户添加系统变量，455版本开始支持
ckjl.addSysVar(1,'sys-字','演示分组','这是我的默认值');
ckjl.addSysVar(2,'sys-数','演示分组','1');
ckjl.addSysVar(3,'sys-坐单','演示分组','500,500');
ckjl.addSysVar(4,'sys-坐多','演示分组','100,200;400,400');
ckjl.addSysVar(5,'sys-图','演示分组');
ckjl.addSysVar(1,'sys-字22');
ckjl.addSysVar(6,'sys-时','演示分组','08:22:58');
```

　

　

　

　

//下面是你可以直接复制拿去运行的代码，注意FlowNexaX版本要455版本及以上

```
        <!DOCTYPE html>
    <html>
    <head>
        <meta charset="UTF-8">
        <title>表单</title>
        <style>
            .dropdown {
                position: relative;
                display: inline-block;
                border: 1px solid #aaa;
                padding: 6px;
                width: 120px;
                cursor: pointer;
            }
            .dropdown-options {
                display: none;
                position: absolute;
                background-color: white;
                border: 1px solid #aaa;
                z-index: 999;
                top: 100%;
                left: 0;
                width: 100%;
            }
            .dropdown-options div {
                padding: 6px;
            }
            .dropdown-options div:hover {
                background-color: #eee;
            }
        </style>
        <script>
        //     // 如果在浏览器调试环境中没有 ckjl，创建一个模拟对象
        // if (typeof ckjl === 'undefined') {
        //     window.ckjl = {
        //         getVar: function (key) {
        //             console.log("ckjl.getVar called with key:", key);
        //             // 根据 key 返回测试数据
        //             if (key === "sys-mmm") return "测试姓名";
        //             if (key === "sys-分") return "male";
        //             if (key === "sys-字") return "游泳,跑步";
        //             if (key === "city") return "上海";
        //             if (key === "sys-图") return "https://via.placeholder.com/100";
        //             return "";
        //         },
        //         setVar: function (key, value) {
        //             console.log("ckjl.setVar called with", key, value);
        //         },
        //         requestImage: function (callbackName) {
        //             console.log("ckjl.requestImage called, will simulate callback...");
        //             // 模拟 1 秒后调用回调函数
        //             setTimeout(() => {
        //                 const fakeImageUrl = "https://via.placeholder.com/100";
        //                 if (typeof window[callbackName] === "function") {
        //                     window[callbackName](fakeImageUrl);
        //                 }
        //             }, 1000);
        //         }
        //     };
        // }

        // 1. 捕获同步错误、引用错误等
        window.onerror = function (message, source, lineno, colno, error) {
            var errorMsg = {
                message: message,
                source: source,
                lineno: lineno,
                colno: colno,
                stack: error && error.stack ? error.stack : null
            };
            if (ckjl && ckjl.onJSError) {
                ckjl.onJSError(JSON.stringify(errorMsg));
            }
            return false; // 不阻止默认行为
        };
        // 2. 捕获未处理的 Promise 错误（如 await 报错）
        window.onunhandledrejection = function (event) {
            const errorInfo = {
                type: "unhandledrejection",
                reason: event.reason?.message || String(event.reason),
                stack: event.reason?.stack || null
            };
            if (ckjl && ckjl.onJsError) {
                ckjl.onJsError(JSON.stringify(errorInfo));
            }
        };

            const cities = ["北京", "上海", "深圳"];
    // sleep 函数（因为你用到了 await sleep）
        function sleep(ms) {
            return new Promise(resolve => setTimeout(resolve, ms));
        }
            function loadData() {
            //演示添加系统变量，type内容为 1代表字符变量，2代表数字变量，3代表坐标单个变量，4代表坐标多个变量，5代表图片变量，6代表时间变量
            ckjl.addSysVar(1,'sys-mmm','演示分组','这是我的默认值');
               ckjl.addSysVar(1,'sys-分','演示分组','女');
               ckjl.addSysVar(1,'sys-字','演示分组','跑步,音乐');
               ckjl.addSysVar(5,'sys-图','演示分组');
               ckjl.addSysVar(3,'sys-坐单','演示分组','500,500');
               ckjl.addSysVar(4,'sys-坐多','演示分组','100,200;400,400');
               ckjl.addSysVar(1,'sys-city','演示分组','上海');
               //以上addSysVar方法需要 FlowNexaX455版本才支持
       
                document.getElementById("name").value = ckjl.getVar("sys-mmm");

                let gender = ckjl.getVar("sys-分");
                let gel=document.getElementById("gender_" + gender);
                if(gel)gel.checked = true;

                let hobbies = ckjl.getVar("sys-字").split(",");
                hobbies.forEach(hobby => {
                    let el = document.getElementById("hobby_" + hobby);
                    if(el)el.checked = true;
                });

                // 回显城市
                let city = ckjl.getVar("sys-city");
                if (city) {
                    document.getElementById("cityDisplay").innerText = city;
                    document.getElementById("cityDropdown").setAttribute("data-value", city);
                }

                let avatar = ckjl.getVar("sys-图");
                if (avatar) {
                    document.getElementById("avatar_img").src = avatar;
                }
            }

            function toggleCityOptions() {
                const options = document.getElementById("cityOptions");
                options.style.display = options.style.display === "block" ? "none" : "block";
            }

            function selectCity(city) {
                document.getElementById("cityDisplay").innerText = city;
                document.getElementById("cityDropdown").setAttribute("data-value", city);
                document.getElementById("cityOptions").style.display = "none";
            }

            function requestAvatar() {
                ckjl.reqScreenshot("sys-图");
            }

            function reqScreenshotBack(vname,url) {
                document.getElementById("avatar_img").src = url;
            }

             function requestPoint() {
                ckjl.reqSelPoint("sys-坐单");
            }

            function reqSelPointBack(vname,pointContent) {
  
                document.getElementById("name").value = pointContent;
            }
              function requestPoints() {
                ckjl.reqSelPoint("sys-坐多");
            }
            function requestPointarea() {
                ckjl.reqSelPoint("sys-坐多","Frame selection of upper left and lower right coordinates");
            }


            async function formCancel() {
            //这是html界面步骤自带的取消按钮点击后会调用你这个formConfirm方法
                document.getElementById("name").value = "点了取消";
                ckjl.formCancelBack();//这是你返回去告诉html界面步骤你处理完了可以关闭
            }

            async function formConfirm() {
            //这是html界面步骤自带的确认按钮点击后会调用你这个formConfirm方法
                document.getElementById("name").value = "点了确认";
                ckjl.formConfirmBack();//这是你返回去告诉html界面步骤你处理完了可以关闭
            }

            function submitForm() {
            //将设置好的内容都设置到变量里面去
                ckjl.setVar("sys-mmm", document.getElementById("name").value);

                let gender = document.querySelector("input[name='gender']:checked");
                if (gender) ckjl.setVar("sys-分", gender.value);

                let selectedHobbies = Array.from(document.querySelectorAll("input[name='hobby']:checked"))
                    .map(el => el.value).join(",");
                ckjl.setVar("sys-字", selectedHobbies);

                let city = document.getElementById("cityDropdown").getAttribute("data-value");
                if (city) ckjl.setVar("sys-city", city);
                //告诉html界面步骤可以确认关闭
                ckjl.formConfirmBack();
            }

            function formZoom() {//缩小界面
               ckjl.formZoom();
             }
     
            function closeWeb() {//直接取消关闭页面
               ckjl.formCancelBack();
             }
               function sureWeb() {//直接确认关闭页面
               ckjl.formConfirmBack();
             }
        </script>
    </head>
    <body onload="loadData()">
    姓名: <input id="name"><br>

    性别:
    <input type="radio" name="gender" id="gender_male" value="male">男
    <input type="radio" name="gender" id="gender_female" value="female">女<br>

    爱好:
    <input type="checkbox" name="hobby" id="hobby_游泳" value="游泳">游泳
    <input type="checkbox" name="hobby" id="hobby_跑步" value="跑步">跑步
    <input type="checkbox" name="hobby" id="hobby_音乐" value="音乐">音乐<br>

    <img id="avatar_img" width="100" height="100"><br>
    <button onclick="requestAvatar()">选择头像</button><br><br>
    <button onclick="requestPoint()">选择坐标</button><br><br>
  
    <button onclick="requestPoints()">选择多坐标</button><br><br>
  
    <button onclick="requestPointarea()">框选坐标</button><br><br>
    城市：
    <div class="dropdown" id="cityDropdown" onclick="toggleCityOptions()" data-value="">
        <span id="cityDisplay">请选择城市</span>
        <div id="cityOptions" class="dropdown-options">
            <div onclick="selectCity('北京')">北京</div>
            <div onclick="selectCity('上海')">上海</div>
            <div onclick="selectCity('深圳')">深圳</div>
        </div>
    </div><br><br>

    <button onclick="submitForm()">提交</button>
  
    <button onclick="closeWeb()">直接关闭</button>
    <button onclick="formZoom()">缩小</button>
    </body>
    </html>
```

### 更多功能正在开发中