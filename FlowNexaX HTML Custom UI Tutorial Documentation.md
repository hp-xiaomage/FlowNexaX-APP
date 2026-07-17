# 💯 This tutorial applies to version 4.4.3 and later 💯

Starting from version 4.4.3, Touch Sprite supports custom interfaces and allows you to customize the display style by writing code.

You need to pay attention to the following methods:

```
The four letters ckjl represent calls to the Touch Sprite API.
// The onJSError method can send errors from the HTML file to Touch Sprite's runtime log.
ckjl.onJSError("Pass the error message to Touch Sprite and write it to the runtime log");

// The getVar method is equivalent to getting the contents of a variable.
ckjl.getVar("系统-字")

// The setVar method sets the contents of a variable.
ckjl.setVar("系统-字", "This content is set to the variable");
// The setVar method sets the contents of a variable. Passing true means the variable content should be persisted. System variables are supported.
ckjl.setVar("系统-字", "This content is set to the variable",true);
// The setVar method sets the contents of a variable. The first true means the variable content should be persisted; the second true means the screenshot position should be stored before saving the image variable. System variables are supported.
ckjl.setVar("系统-图", "image variable address",true,true);

// The reqScreenshot method means you want to take a screenshot for the 系统-图 variable.
ckjl.reqScreenshot("系统-图"); // If no screenshot type is passed, it defaults to screenshot.
ckjl.reqScreenshot("系统-图","截图");
ckjl.reqScreenshot("系统-图","截图并记录位置");
ckjl.reqScreenshot("系统-图","不规则截图");
ckjl.reqScreenshot("系统-图","不规则截图并记录位置");


// The reqScreenshotBack method is used together with reqScreenshot. It indicates that after you successfully take a screenshot for the 系统-图 variable, reqScreenshotBack will notify you that the screenshot succeeded and pass the screenshot image URL to you. vname is the variable name you used earlier in reqScreenshot("系统-图"), namely 系统-图.
function reqScreenshotBack(vname,url){}

// The reqSelPoint method means you want to select a coordinate on the screen for the 系统-坐单 variable.
ckjl.reqSelPoint("系统-坐单");
// The reqSelPoint method means you want to select the upper-left and lower-right coordinates by drawing a rectangle on the screen for the multiple-coordinate variable 系统-坐多. Supported starting from version 4.4.9.
ckjl.reqSelPoint("系统-坐多","框选左上和右下坐标");
// The reqSelPoint method means you want to select multiple coordinates on the screen for the multiple-coordinate variable 系统-坐多. Supported starting from version 4.4.9.
ckjl.reqSelPoint("系统-坐多");

// The reqSelPointBack method is used together with reqSelPoint. It indicates that after you successfully select a coordinate on the screen for the 系统-坐单 variable, reqSelPointBack will notify you that the selection succeeded and pass the coordinate content pointContent to you. vname is the variable name you used earlier in reqSelPoint("系统-坐单"), namely 系统-坐单.
function reqSelPointBack(vname,pointContent){}


// formCancel is a supporting method for the custom HTML interface. When the Cancel button in the interface is clicked, this method is called. ckjl.formCancelBack() inside it is required. You must call ckjl.formCancelBack(); otherwise, the dialog interface will not close.
            async function formCancel() {
                document.getElementById("name").value = "Cancel clicked";
                await sleep(3000);
                ckjl.formCancelBack();
            }


// formConfirm is a supporting method for the custom HTML interface. When the Confirm button in the interface is clicked, this method is called. ckjl.formConfirmBack() inside it is required. You must call ckjl.formConfirmBack(); otherwise, the dialog interface will not close.
            async function formConfirm() {
                document.getElementById("name").value = "Confirm clicked";
                await sleep(3000);
                ckjl.formConfirmBack();
            }


// The formZoom method minimizes the entire interface. Supported starting from version 4.5.5.
ckjl.formZoom();

// The addSysVar method adds system variables. Supported starting from version 4.5.5.
ckjl.addSysVar(1,'系统-字','Demo Group','This is my default value');
ckjl.addSysVar(2,'系统-数','Demo Group','1');
ckjl.addSysVar(3,'系统-坐单','Demo Group','500,500');
ckjl.addSysVar(4,'系统-坐多','Demo Group','100,200;400,400');
ckjl.addSysVar(5,'系统-图','Demo Group');
ckjl.addSysVar(1,'系统-字22');
ckjl.addSysVar(6,'系统-时','Demo Group','08:22:58');
```

　

　

　

　

// The following code can be copied directly and run. Note that the Touch Sprite version must be 4.5.5 or later.

```
        <!DOCTYPE html>
    <html>
    <head>
        <meta charset="UTF-8">
        <title>Form</title>
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
        //     // If ckjl is not available in the browser debugging environment, create a mock object.
        // if (typeof ckjl === 'undefined') {
        //     window.ckjl = {
        //         getVar: function (key) {
        //             console.log("ckjl.getVar called with key:", key);
        //             // Return test data according to key.
        //             if (key === "系统-mmm") return "Test Name";
        //             if (key === "系统-分") return "male";
        //             if (key === "系统-字") return "swimming,running";
        //             if (key === "city") return "Shanghai";
        //             if (key === "系统-图") return "https://via.placeholder.com/100";
        //             return "";
        //         },
        //         setVar: function (key, value) {
        //             console.log("ckjl.setVar called with", key, value);
        //         },
        //         requestImage: function (callbackName) {
        //             console.log("ckjl.requestImage called, will simulate callback...");
        //             // Simulate calling the callback function after 1 second.
        //             setTimeout(() => {
        //                 const fakeImageUrl = "https://via.placeholder.com/100";
        //                 if (typeof window[callbackName] === "function") {
        //                     window[callbackName](fakeImageUrl);
        //                 }
        //             }, 1000);
        //         }
        //     };
        // }

        // 1. Capture synchronous errors, reference errors, etc.
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
            return false; // Do not prevent the default behavior.
        };
        // 2. Capture unhandled Promise errors, such as errors thrown by await.
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

            const cities = ["Beijing", "Shanghai", "Shenzhen"];
    // sleep function, because await sleep is used.
        function sleep(ms) {
            return new Promise(resolve => setTimeout(resolve, ms));
        }
            function loadData() {
            // Demonstrate adding system variables. type values: 1 = string variable, 2 = number variable, 3 = single-coordinate variable, 4 = multiple-coordinate variable, 5 = image variable, 6 = time variable.
            ckjl.addSysVar(1,'系统-mmm','Demo Group','This is my default value');
               ckjl.addSysVar(1,'系统-分','Demo Group','female');
               ckjl.addSysVar(1,'系统-字','Demo Group','running,music');
               ckjl.addSysVar(5,'系统-图','Demo Group');
               ckjl.addSysVar(3,'系统-坐单','Demo Group','500,500');
               ckjl.addSysVar(4,'系统-坐多','Demo Group','100,200;400,400');
               ckjl.addSysVar(1,'系统-city','Demo Group','Shanghai');
               // The addSysVar method above requires Touch Sprite version 4.5.5.
       
                document.getElementById("name").value = ckjl.getVar("系统-mmm");

                let gender = ckjl.getVar("系统-分");
                let gel=document.getElementById("gender_" + gender);
                if(gel)gel.checked = true;

                let hobbies = ckjl.getVar("系统-字").split(",");
                hobbies.forEach(hobby => {
                    let el = document.getElementById("hobby_" + hobby);
                    if(el)el.checked = true;
                });

                // Echo the city.
                let city = ckjl.getVar("系统-city");
                if (city) {
                    document.getElementById("cityDisplay").innerText = city;
                    document.getElementById("cityDropdown").setAttribute("data-value", city);
                }

                let avatar = ckjl.getVar("系统-图");
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
                ckjl.reqScreenshot("系统-图");
            }

            function reqScreenshotBack(vname,url) {
                document.getElementById("avatar_img").src = url;
            }

             function requestPoint() {
                ckjl.reqSelPoint("系统-坐单");
            }

            function reqSelPointBack(vname,pointContent) {
  
                document.getElementById("name").value = pointContent;
            }
              function requestPoints() {
                ckjl.reqSelPoint("系统-坐多");
            }
            function requestPointarea() {
                ckjl.reqSelPoint("系统-坐多","框选左上和右下坐标");
            }


            async function formCancel() {
            // This formCancel method is called after the built-in Cancel button of the HTML interface step is clicked.
                document.getElementById("name").value = "Cancel clicked";
                ckjl.formCancelBack();// This tells the HTML interface step that processing is complete and it can close.
            }

            async function formConfirm() {
            // This formConfirm method is called after the built-in Confirm button of the HTML interface step is clicked.
                document.getElementById("name").value = "Confirm clicked";
                ckjl.formConfirmBack();// This tells the HTML interface step that processing is complete and it can close.
            }

            function submitForm() {
            // Set all configured contents into variables.
                ckjl.setVar("系统-mmm", document.getElementById("name").value);

                let gender = document.querySelector("input[name='gender']:checked");
                if (gender) ckjl.setVar("系统-分", gender.value);

                let selectedHobbies = Array.from(document.querySelectorAll("input[name='hobby']:checked"))
                    .map(el => el.value).join(",");
                ckjl.setVar("系统-字", selectedHobbies);

                let city = document.getElementById("cityDropdown").getAttribute("data-value");
                if (city) ckjl.setVar("系统-city", city);
                // Tell the HTML interface step that it can confirm and close.
                ckjl.formConfirmBack();
            }

            function formZoom() {// Minimize the interface.
               ckjl.formZoom();
             }
     
            function closeWeb() {// Cancel directly and close the page.
               ckjl.formCancelBack();
             }
               function sureWeb() {// Confirm directly and close the page.
               ckjl.formConfirmBack();
             }
        </script>
    </head>
    <body onload="loadData()">
    Name: <input id="name"><br>

    Gender:
    <input type="radio" name="gender" id="gender_male" value="male">Male
    <input type="radio" name="gender" id="gender_female" value="female">Female<br>

    Hobbies:
    <input type="checkbox" name="hobby" id="hobby_游泳" value="游泳">Swimming
    <input type="checkbox" name="hobby" id="hobby_跑步" value="跑步">Running
    <input type="checkbox" name="hobby" id="hobby_音乐" value="音乐">Music<br>

    <img id="avatar_img" width="100" height="100"><br>
    <button onclick="requestAvatar()">Select Avatar</button><br><br>
    <button onclick="requestPoint()">Select Coordinate</button><br><br>
  
    <button onclick="requestPoints()">Select Multiple Coordinates</button><br><br>
  
    <button onclick="requestPointarea()">Select Coordinate Area</button><br><br>
    City:
    <div class="dropdown" id="cityDropdown" onclick="toggleCityOptions()" data-value="">
        <span id="cityDisplay">Please select a city</span>
        <div id="cityOptions" class="dropdown-options">
            <div onclick="selectCity('北京')">Beijing</div>
            <div onclick="selectCity('上海')">Shanghai</div>
            <div onclick="selectCity('深圳')">Shenzhen</div>
        </div>
    </div><br><br>

    <button onclick="submitForm()">Submit</button>
  
    <button onclick="closeWeb()">Close Directly</button>
    <button onclick="formZoom()">Minimize</button>
    </body>
    </html>
```

### More features are under development