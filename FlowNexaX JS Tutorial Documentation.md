# 💯 This tutorial applies to version 3.8.1 and later 💯

Starting from version 3.8.1, Touch Sprite upgraded its engine and supports more standard syntax.

# Running JS Code

By writing JS code, you can implement the most flexible script logic control and enhanced extensibility.

> Writing JS code requires basic knowledge of the JavaScript language. You can search online to learn it, for example:
>
> * JavaScript Language Introduction Tutorial: [https://wangdoc.com/javascript/](https://wangdoc.com/javascript/)
> * JavaScript Tutorial: [https://www.runoob.com/js/js-tutorial.html](https://www.runoob.com/js/js-tutorial.html)
> * JS Beginner Tutorial: [http://c.biancheng.net/js/](http://c.biancheng.net/js/)

In addition to the general capabilities of the JS engine, Touch Sprite also extends the following capabilities:

### Recommendations

```js

Writing JS code on a mobile phone is relatively troublesome. It is recommended to write it on a computer and then copy it into Touch Sprite. Long-press the JS editor box in the app; there is a PASTE button and other buttons such as undo, redo, cut, copy, and select all.

Of course, for writing code, it is still not suitable to write on a mobile phone. Please note that when writing code in the app, using a Chinese input method may delete the previous character while typing. This is an input method issue, so it is still recommended to write it on a computer and send it to the phone.

The editor box in the app will help you check whether the JS syntax is correct. Warning icons appear next to the line numbers on the left.

```

### General APIs

```js

// Get the current Touch Sprite app version number, such as "3.4.0".
var version=getAppVersion();
log(version);

// Set the current system clipboard text.
var result=setClipboard('三生三世');
log(result);

// Get the current system clipboard text.
var content=getClipboard();
log(content);

// Show a simple toast that disappears automatically.
toast('Prompt'); 

// Wait 1 second.
sleep(1000); 

```

### Global and System Variables

```js

// Read variable a from system variables or global variables.
var variable=getVar('系统-坐1');
log(variable);

// Write the value of variable 系统-偷卡 as the string "aaa".
setVar('系统-偷卡', 'aaa');

// Starting from version 3.9.6, system variables support configuring whether to persist. Write the value of variable 系统-偷卡 as the string "aaa"; true means persist it.
setVar('系统-偷卡', 'aaa', true);

// Read all variables from global variables.
var vars=getVars();
log(vars);// Print all variables as a JSON string in the log.
var varsObj= JSON.parse(vars);// Format the variable JSON.
var content=varsObj['坐标a']['vcontent'];// Extract the content of variable 坐标a.


// Read all variables from system variables.
var sysVars=getSysVars();
log(sysVars);// Print all variables as a JSON string in the log.
var sysVarsObj= JSON.parse(sysVars);// Format the variable JSON.
var content=sysVarsObj['系统-坐1']['vcontent'];// Extract the content of variable 系统-坐1.

// Version 4.5.5 supports adding system variables with addSysVar.
addSysVar({
  type: 1, // 1 means string, 2 means number, 3 means single coordinate, 4 means multiple coordinates, 5 means image, 6 means time.
  name: '系统-我的变量名字', // Variable name.
  group: '我的分组', // Variable group name.
  defValue: 'This is my default value', // Default value.
});


```

### Print Logs

Used to output the information you want in the runtime log panel.

```js

log("Log information"); // Output a log to the runtime log panel.

clearLog(); // Clear the runtime log.

```

### Step Actions

Same as the steps you add or record.

```js

// Click step.

click(x, y, duration); // Click the coordinate position x,y. duration is the click duration in milliseconds.

click('50%', '50%'); // Click the center of the screen.

click('200dp', '100dp'); // Click the logical pixel position 200dp, 100dp on the screen.

click(300, 500, 1000); // Long-press the screen pixel position 300,500 for 1000 milliseconds (1 second).

longClick('50%', '50%'); // Long-press the center of the screen, same as ckjl.click('50%', '50%', 600).


// Swipe step:

swipe(x1, y1, x2, y2, duration); // Swipe from x1,y1 to x2,y2. duration is the swipe duration in milliseconds.

swipe('50%', '50%', '50%', '10%', 1000); // Swipe upward from the center of the screen. Duration: 1000 milliseconds (50%,50% -> 50%,10%).


// Single-finger gesture:

gesture(duration, [x1, y1], [x2, y2], ...); // Gesture swipe. The ellipsis means you can add unlimited points.

gesture(2000, ['50%', '10%'], ['50%', '50%'], ['80%', '50%']); // Gesture swipe (50%,10% -> 50%,50% -> 80%,50%), duration 2000 milliseconds (2 seconds). Pixels or dp are also supported.


// Multi-finger operation:

gestures([delay1, duration1, [x1, y1], [x2, y2], ...], [delay2, duration2, [x3, y3], [x4, y4], ...], ...); // Multi-finger gestures. Each gesture parameter is [delay, duration, coordinate1, coordinate2, ...]. delay is how long to wait (milliseconds) before executing the gesture; duration is the gesture execution duration (milliseconds); coordinates are the points the gesture passes through.

gestures([0, 2000, ['80%', '20%'], ['50%', '50%']],
              [0, 2000, ['20%', '80%'], ['50%', '50%']]); // Two-finger pinch gesture. Pixels or dp are also supported.

```

Supported coordinate units:

* Physical pixel coordinates, such as: (100, 100).
* Logical pixel coordinates, such as: ('200dp', '100dp').
* Screen percentage coordinates, such as: ('50%', '30%'). Percentage coordinates are recommended to ensure multi-screen adaptation.

### Find Coordinates

You can find coordinates on the screen according to the conditions passed in. (The upper-left corner of the screen is the origin of the coordinate axis. The x-axis goes from left to right, and the y-axis goes from top to bottom.)

```js
// Find one text coordinate on the screen.
var result= findLocation({
  type: 'nodetext', // Control text type.
  text: '文本', // Text content. Regular expressions are also supported. For example, writing 正则@[\\d+] means matching numbers.
  left: '20%', // Limit the left side of the search area. Pixels, dp, and percentages are supported.
  top: '20dp', // Limit the top side of the search area. Pixels, dp, and percentages are supported.
  right: '800', // Limit the right side of the search area. Pixels, dp, and percentages are supported.
  bottom: '60%', // Limit the bottom side of the search area. Pixels, dp, and percentages are supported.
  like: true,  // Fill true for fuzzy matching; false for exact matching.
  sim: 100,  // Text similarity percentage. Fill 100 to require the text to be exactly identical.
});
var location = JSON.parse(result);// Format as JSON.
log('Text center x coordinate (pixels)'+ location);
log('Text center x coordinate (pixels)'+ location.x);
log('Text center y coordinate (pixels)'+ location.y);
log('Text left coordinate (pixels)'+ location.left);
log('Text top coordinate (pixels)'+ location.top);
log('Text right coordinate (pixels)'+ location.right);
log('Text bottom coordinate (pixels)'+ location.bottom);




// Find multiple text coordinates on the screen.
var result = findLocation({
  type: 'nodetext', // Control text type.
  text: '8', // Text content. Regular expressions are also supported. For example, writing 正则@[\\d+] means matching numbers.
  left: '20%', // Limit the left side of the search area. Pixels, dp, and percentages are supported.
  top: '20dp', // Limit the top side of the search area. Pixels, dp, and percentages are supported.
  right: '800', // Limit the right side of the search area. Pixels, dp, and percentages are supported.
  bottom: '60%', // Limit the bottom side of the search area. Pixels, dp, and percentages are supported.
  like: true,  // Fill true for fuzzy matching; false for exact matching.
  sim: 0,  // Text similarity percentage. Fill 0 to ignore this configuration.
},true);  // The true here means finding all positions.
var location = JSON.parse(result);// Format as JSON.
for (var i=0;i<location.length;i++)
{ 
    log('Text center x coordinate (pixels)'+ location[i].x);
    log('Text center y coordinate (pixels)'+ location[i].y);
    log('Text left coordinate (pixels)'+ location[i].left);
    log('Text top coordinate (pixels)'+ location[i].top);
    log('Text right coordinate (pixels)'+ location[i].right);
    log('Text bottom coordinate (pixels)'+ location[i].bottom);
}




// Find one text coordinate on the screen (OCR recognition).
var result = findLocation({
  type: 'ocrtext', // OCR text recognition type.
  text: '6', // Text content. Regular expressions are also supported. For example, writing 正则@[\\d+] means matching numbers.
  left: '20%', // Limit the left side of the search area. Pixels, dp, and percentages are supported.
  top: '20dp', // Limit the top side of the search area. Pixels, dp, and percentages are supported.
  right: '1000', // Limit the right side of the search area. Pixels, dp, and percentages are supported.
  bottom: '90%', // Limit the bottom side of the search area. Pixels, dp, and percentages are supported.
  like: true,  // Fill true for fuzzy matching; false for exact matching.
  pre: '511', // Use the original image.
  sim: 80,  // Text similarity percentage. Fill 80 to require recognized text content to reach 80% similarity.
});
var location = JSON.parse(result);
log('Text center x coordinate (pixels)'+ location.x);
log('Text center y coordinate (pixels)'+ location.y);
log('Text left coordinate (pixels)'+ location.left);
log('Text top coordinate (pixels)'+ location.top);
log('Text right coordinate (pixels)'+ location.right);
log('Text bottom coordinate (pixels)'+ location.bottom);




// Find multiple text coordinates on the screen (OCR recognition).
var result = findLocation({
  type: 'ocrtext', // OCR text recognition type.
  text: '文本', // Text content. Regular expressions are also supported. For example, writing 正则@[\\d+] means matching numbers.
  left: '20%', // Limit the left side of the search area. Pixels, dp, and percentages are supported.
  top: '20dp', // Limit the top side of the search area. Pixels, dp, and percentages are supported.
  right: '800', // Limit the right side of the search area. Pixels, dp, and percentages are supported.
  bottom: '60%', // Limit the bottom side of the search area. Pixels, dp, and percentages are supported.
  like: true,  // Fill true for fuzzy matching; false for exact matching.
  pre: '504', // Extract red.
  sim: 80,  // Text similarity percentage. Fill 80 to require recognized text content to reach 80% similarity.
},true);// The true here means finding all positions.
var location = JSON.parse(result);// Format as JSON.
for (var i=0;i<location.length;i++)
{ 
    log('Text center x coordinate (pixels)'+ location[i].x);
    log('Text center y coordinate (pixels)'+ location[i].y);
    log('Text left coordinate (pixels)'+ location[i].left);
    log('Text top coordinate (pixels)'+ location[i].top);
    log('Text right coordinate (pixels)'+ location[i].right);
    log('Text bottom coordinate (pixels)'+ location[i].bottom);
}

// Find multiple text coordinates on the screen (OCR recognition).
var result = findLocation({
  type: 'ocrtext', // OCR text recognition type.
  text: '文本', // Text content. Regular expressions are also supported. For example, writing 正则@[\\d+] means matching numbers.
  left: '20%', // Limit the left side of the search area. Pixels, dp, and percentages are supported.
  top: '20dp', // Limit the top side of the search area. Pixels, dp, and percentages are supported.
  right: '800', // Limit the right side of the search area. Pixels, dp, and percentages are supported.
  bottom: '60%', // Limit the bottom side of the search area. Pixels, dp, and percentages are supported.
  like: true,  // Fill true for fuzzy matching; false for exact matching.
  pre: '512', // Extract a custom color. Custom colors require version 4.3.8.
  r: 3, // Required for extracting a custom color. rgb is the color value; search Baidu for RGB color values.
  g: 202, // Required for extracting a custom color. rgb is the color value; search Baidu for RGB color values.
  b: 99, // Required for extracting a custom color. rgb is the color value; search Baidu for RGB color values.
  sim: 80,  // Text similarity percentage. Fill 80 to require recognized text content to reach 80% similarity.
},true);// The true here means finding all positions.
var location = JSON.parse(result);// Format as JSON.
for (var i=0;i<location.length;i++)
{ 
    log('Text center x coordinate (pixels)'+ location[i].x);
    log('Text center y coordinate (pixels)'+ location[i].y);
    log('Text left coordinate (pixels)'+ location[i].left);
    log('Text top coordinate (pixels)'+ location[i].top);
    log('Text right coordinate (pixels)'+ location[i].right);
    log('Text bottom coordinate (pixels)'+ location[i].bottom);
}

The configurable extracted colors for pre above are:
GET_BLACK(501,"Extract black"),
GET_GRAY(502,"Extract gray"),
GET_WHITE(503,"Extract white"),
GET_RED(504,"Extract red"),
GET_ORANGE(505,"Extract orange"),
GET_YELLOW(506,"Extract yellow"),
GET_GREEN(507,"Extract green"),
GET_CYAN(508,"Extract cyan"),
GET_BLUE(509,"Extract blue"),
GET_PURPLE(510,"Extract purple"),
GET_YUANTU(511,"Use original image"),
GET_COUSTOM(512,"Extract custom color"),





// Find multiple control coordinates on the screen.
var result = findLocation({
  type: 'node', // Control type.
  className: 'android.widget.TextView',  // Control class name.
  nodeText: '摄影摄像',  // Control text. Regular expressions are also supported. For example, writing 正则@[\\d+] means matching numbers.
  nodeDes: '描述',  // Control description.
  nodeId: 'icon_title',  // Control ID.
  packageName: 'com.tengxun',  // Control package name.
  scrollable: true,  // Whether the control is scrollable. true = yes, false = no.
  clickable: false,  // Whether the control is clickable.
  longClickable: true,  // Whether the control can be long-pressed.
  left: '20%', // Limit the left side of the search area. Pixels, dp, and percentages are supported.
  top: '20dp', // Limit the top side of the search area. Pixels, dp, and percentages are supported.
  right: '800', // Limit the right side of the search area. Pixels, dp, and percentages are supported.
  bottom: '60%', // Limit the bottom side of the search area. Pixels, dp, and percentages are supported.
},true);  // The true here means finding all positions; false means finding only one.
var location = JSON.parse(result);
for (var i=0;i<location.length;i++)
{ 
    log('Control center x coordinate (pixels)', location[i].x);
    log('Control center y coordinate (pixels)', location[i].y);
    log('Control left coordinate (pixels)', location[i].left);
    log('Control top coordinate (pixels)', location[i].top);
    log('Control right coordinate (pixels)', location[i].right);
    log('Control bottom coordinate (pixels)', location[i].bottom);
}

```

### Find Controls

You can find controls on the screen according to the conditions passed in.

```js
// Find one control on the screen.
var result = findNode({
  className: 'android.widget.TextView',  // Control class name.
  nodeText: '摄影摄像',  // Control text. Regular expressions are also supported. For example, writing 正则@[\\d+] means matching numbers.
  nodeDes: '描述',  // Control description.
  nodeId: 'icon_title',  // Control ID.
  packageName: 'com.tengxun',  // Control package name.
  scrollable: true,  // Whether the control is scrollable. true = yes, false = no.
  clickable: false,  // Whether the control is clickable.
  longClickable: true,  // Whether the control can be long-pressed.
});
var node=JSON.parse(result);
log('Control text'+node.nodeText);
log('Control class name'+ node.className);
log('Control package name'+ node.packageName);
log('Control ID'+ node.nodeId);
log('Control description'+ node.nodeDes);
log('Whether the control can be long-pressed'+ node.isLongClickable);
log('Whether the control is scrollable'+ node.isScrollable);
log('Whether the control is clickable'+ node.isClickable);
log('Distance from the control left border to the left side of the screen'+node.left);
log('Distance from the control top border to the top of the screen'+ node.top);
log('Distance from the control right border to the left side of the screen'+ node.right);
log('Distance from the control bottom border to the top of the screen'+node.bottom);



// Find multiple controls on the screen.
var result = findNode({
  className: 'android.widget.TextView',  // Control class name.
  nodeText: '摄影摄像',  // Control text. Regular expressions are also supported. For example, writing 正则@[\\d+] means matching numbers.
  nodeDes: '描述',  // Control description.
  nodeId: 'icon_title',  // Control ID.
  packageName: 'com.tengxun',  // Control package name.
  scrollable: true,  // Whether the control is scrollable. true = yes, false = no.
  clickable: false,  // Whether the control is clickable.
  longClickable: true,  // Whether the control can be long-pressed.
},true);// true means finding all matching controls.
var node=JSON.parse(result);
for (var i=0;i<node.length;i++)
{ 
  log('-------------------------'+i);
  log('Control text'+node[i].nodeText);
  log('Control class name'+ node[i].className);
  log('Control package name'+ node[i].packageName);
  log('Distance from the control left border to the left side of the screen'+node[i].left);
  log('Distance from the control top border to the top of the screen'+ node[i].top);
  log('Distance from the control right border to the left side of the screen'+ node[i].right);
  log('Distance from the control bottom border to the top of the screen'+node[i].bottom);
}





// Find multiple controls on the screen and their child controls.
var result = findNode({
  className: 'android.widget.RelativeLayout',
  left: '20%', // Limit the left side of the search area. Pixels, dp, and percentages are supported.
  top: '20dp', // Limit the top side of the search area. Pixels, dp, and percentages are supported.
  right: '800', // Limit the right side of the search area. Pixels, dp, and percentages are supported.
  bottom: '60%', // Limit the bottom side of the search area. Pixels, dp, and percentages are supported.
},true,true);// The first true means finding multiple controls; the second true means finding child controls of the controls.
var node=JSON.parse(result);
for (var i=0;i<node.length;i++)
{ 
  log('-------------------------'+i);

  log('Control class name'+ node[i].className);
  if(node[i].children){
     var childlength=node[i].children.length;
     log('Number of child controls'+ childlength);
    for (var y=0;y<childlength;y++)
    { 
       var childlist=node[i].children[y];
       log('Child control class name'+ childlist.className);
    } 

  }

}



```

### Screen Recognition

You can recognize control text at a specified location on the screen, or recognize text inside an image.

```js

// Recognize text controls on the screen and extract text.
var result = getScreenText({
  type: 'nodetext',  // Recognize text controls.
  left: '20%', // Limit the left side of the search area. Pixels, dp, and percentages are supported.
  top: '20dp', // Limit the top side of the search area. Pixels, dp, and percentages are supported.
  right: '800', // Limit the right side of the search area. Pixels, dp, and percentages are supported.
  bottom: '60%', // Limit the bottom side of the search area. Pixels, dp, and percentages are supported.
});
var texts=JSON.parse(result);
for (var i=0;i<texts.length;i++)
{ 
  log('-------------------------'+i);
  log('Text content: '+ texts[i].text);
  log('Text position: left: '+ texts[i].left);
  log('Text position: top: '+ texts[i].top);
  log('Text position: right: '+ texts[i].right);
  log('Text position: bottom: '+ texts[i].bottom);

}





// Recognize text on the screen (OCR recognition).
var result = getScreenText({
  type: 'ocrtext',  // Recognize text controls.
  left: '20%', // Limit the left side of the search area. Pixels, dp, and percentages are supported.
  top: '20dp', // Limit the top side of the search area. Pixels, dp, and percentages are supported.
  right: '800', // Limit the right side of the search area. Pixels, dp, and percentages are supported.
  bottom: '60%', // Limit the bottom side of the search area. Pixels, dp, and percentages are supported.
  pre: '511', // Extract red.
});
var texts=JSON.parse(result);
for (var i=0;i<texts.length;i++)
{ 
  log('-------------------------'+i);
  log('Text content: '+ texts[i].text);
  log('Text position: left: '+ texts[i].left);
  log('Text position: top: '+ texts[i].top);
  log('Text position: right: '+ texts[i].right);
  log('Text position: bottom: '+ texts[i].bottom);

}

// Recognize text on the screen (OCR recognition).
var result = getScreenText({
  type: 'ocrtext',  // Recognize text controls.
  left: '20%', // Limit the left side of the search area. Pixels, dp, and percentages are supported.
  top: '20dp', // Limit the top side of the search area. Pixels, dp, and percentages are supported.
  right: '800', // Limit the right side of the search area. Pixels, dp, and percentages are supported.
  bottom: '60%', // Limit the bottom side of the search area. Pixels, dp, and percentages are supported.
  pre: '512', // Extract a custom color. Custom colors require version 4.3.8.
  r: 3, // Required for extracting a custom color. rgb is the color value; search Baidu for RGB color values.
  g: 202, // Required for extracting a custom color. rgb is the color value; search Baidu for RGB color values.
  b: 99, // Required for extracting a custom color. rgb is the color value; search Baidu for RGB color values.
});
var texts=JSON.parse(result);
for (var i=0;i<texts.length;i++)
{ 
  log('-------------------------'+i);
  log('Text content: '+ texts[i].text);
  log('Text position: left: '+ texts[i].left);
  log('Text position: top: '+ texts[i].top);
  log('Text position: right: '+ texts[i].right);
  log('Text position: bottom: '+ texts[i].bottom);

}

The configurable extracted colors for pre above are:
GET_BLACK(501,"Extract black"),
GET_GRAY(502,"Extract gray"),
GET_WHITE(503,"Extract white"),
GET_RED(504,"Extract red"),
GET_ORANGE(505,"Extract orange"),
GET_YELLOW(506,"Extract yellow"),
GET_GREEN(507,"Extract green"),
GET_CYAN(508,"Extract cyan"),
GET_BLUE(509,"Extract blue"),
GET_PURPLE(510,"Extract purple"),
GET_YUANTU(511,"Use original image");
GET_YUANTU(512,"Extract custom color");



// Extract the color value at the specified coordinate position.
var result = getScreenColor('20%','20dp'); // Search position. Pixels, dp, and percentages are supported.
var col=JSON.parse(result);
log('Hex color value content: '+ col.hexColor);
log('Color value content: '+ col.color);
log('Color value content after clearing the Alpha channel: '+ col.unsignedColor);



// Example of sending a request.
// Construct the request body object.
var requestBodyObj = {
    "user_id": "your account",
    "messages": [
        {
            "content": "message",
            "content_type": "text",
            "role": "user",
            "type": "question"
        }
    ]
};
var headerParamObj = {
        'Authorization': "Bearer your token",
        'Content-Type': 'application/json'
    };

var configParamObj = {
        connectTimeout: 60,
        readTimeout: 60,
        writeTimeout: 60
    } ;

// Serialize to JSON strings.
var requestBodyStr = JSON.stringify(requestBodyObj);
var headerParamStr = JSON.stringify(headerParamObj);
var configParamStr = JSON.stringify(configParamObj);

// Send the request.
var result = requestUrl({ 
    url: 'http://www.baidu.com', 
    method: 'POST', 
    // Directly pass the serialized string.
    inputParam: requestBodyStr, // Request body input parameter.
    headerParam: headerParamStr, // Request header input parameter.
    configParam: configParamStr  // Request configuration input parameter, such as timeout duration.
}); 
var content=JSON.parse(result);
log('Request returned content: '+ content);
```

### Features supported starting from version 3.8.5:

### URL scheme jump to target page

You can open a specified page in the target app.

```js

// Use a URL scheme to open a specified page in the target app. You can directly open a live page or other page in apps such as Kuaishou and Douyin, provided that Kuaishou and Douyin support opening via intent behavior.
startActivity({
     data: "target app URL scheme",
});
Example: open a specified QQ chat interface:
startActivity({
     data: "mqqwpa://im/chat?chat_type=wpa&uin=Zhang San's QQ number",
});

Links for jumping in common apps such as Douyin and Kuaishou:
https://blog.csdn.net/qq_23857415/article/details/134778701?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EYuanLiJiHua%7EPosition-3-134778701-blog-108361386.235%5Ev43%5Epc_blog_bottom_relevance_base7&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EYuanLiJiHua%7EPosition-3-134778701-blog-108361386.235%5Ev43%5Epc_blog_bottom_relevance_base7&utm_relevant_index=4
```

　

### Dialog Features

You can pop up a confirmation dialog.

```js

alert("Dialog prompt content");// Synchronous dialog by default. It waits until Confirm is clicked before continuing.

alert("Dialog prompt content",false);// false means asynchronous popup, which does not block subsequent code execution.


confirm("Dialog prompt content");// Dialog with Confirm and Cancel buttons. Synchronous dialog by default.

confirm("Dialog prompt content",false);// Dialog with Confirm and Cancel buttons. false means asynchronous dialog.


var result=confirm("Dialog prompt content","Agree","Disagree"); // Dialog with custom button labels. Synchronous dialog by default. result returns which button was clicked.
log(result);

confirm("Dialog prompt content","Agree","Disagree",false);// Dialog with custom button labels. false means asynchronous dialog. result returns which button was clicked.


```

　

　

### Features supported starting from version 4.0.2:

### Execute ADB Commands on Rooted Phones

You can execute root commands.

```js

runRoot("your root command");

For example:
// Swipe from screen x,y coordinate 100,100 to screen x,y coordinate 400,400, taking 1 second.
runRoot("input swipe 100 100 400 400 1000");

// Tap screen x,y coordinate 100,100, with a tap duration of 1 second.
runRoot("input swipe 100 100 100 100 1000");

// Double-tap screen x,y coordinate 100,100, with a double-tap interval of 0.14 seconds.
runRoot("seq 2 | while read i;do input tap 100 100 & input tap 100 100 & sleep 0.14;done");

// Press the Back key.
runRoot("input keyevent 4");

// Press the Home key.
runRoot("input keyevent 3");

// Press the Recent Apps key.
runRoot("input keyevent 82");

// Press the Lock Screen key.
runRoot("input keyevent 26");

For many more commands, search Baidu for: common Android adb commands.

Features supported starting from version 4.3.7:
var result=adbWithResult("your root command");
With the adbWithResult method, you can get the result returned after the adb command is executed.


```

　

　

### Features supported starting from version 4.2.7:

### Execute HID Commands; Supports Shanling, Fengqun, and Rain

You can execute HID keycode commands.

```js

keyPressCode("keycode number");

For example:
Shanling keycode reference:
Address: https://vimsky.com/examples/usage/arduino-language-functions-usb-keyboard-keyboardmodifiers-ar.html
![img](ckjl/1735783840868_101.png)
![img](ckjl/[银行卡]_102.png)
![img](ckjl/1735783864959_103.png)
![img](ckjl/1735783874711_104.png)
![img](ckjl/1735783893997_105.png)
![img](ckjl/1735783902327_106.png)
![img](ckjl/[银行卡]_107.png)


// Fengqun keycode reference:
● Keycode reference address: https://blog.csdn.net/u011119684/article/details/124978540
● Must be used together with a server connection.

```

　

### Features supported starting from version 4.6.7:

```js

// Basic link opening.
openLink({ data: "https://www.baidu.com" });


// Single parameter: only pass the key name.
pcClickKey("enter");
pcClickKey("escape");
pcClickKey("space");

// Two parameters: key name + duration (ms).
pcClickKey("ctrl+c", 100);
pcClickKey("tab", 200);
pcClickKey("backspace", 50);

// Two parameters: x coordinate, y coordinate (as strings).
pcClickRight("500", "300");

// Three parameters: x coordinate, y coordinate, duration (ms).
pcClickRight("500", "300", 100);
pcClickRight("100", "200", 200);

// Back key.
back();

// Return to home screen.
home();

// Recent apps.
recents();

// Pull down notification shade.
notifications();

// Pull down quick settings.
quickSettings();

// Power menu.
powerDialog();

// Lock screen.
lockScreen();

// Screenshot.
takeScreenshot();

// Wake screen and unlock.
wakeUpAndUnlock();

// Enable airplane mode. Only effective on rooted phones.
enableAirplaneMode();

// Disable airplane mode. Only effective on rooted phones.
disableAirplaneMode();


// Speak text while tapping a pixel coordinate. If coordinates are not filled, no tap is performed. duration represents how long to press; if not filled, the press duration is automatically based on the length of the spoken text. delay represents how long to wait before speaking, but it does not affect the press: the press still starts immediately, while the speech starts after 2000 ms.
speakText("你好", {x: '540', y: '960', duration: 3000, delay: 2000})
speakText("你好", {x: '50%', y: '70%', duration: 3000})
speakText("你好", {x: '540', y: '960', delay: 2000})
speakText("你好", {x: '50%', y: '80%'})

// Play network audio while tapping a coordinate. If coordinates are not filled, no tap is performed. Playback starts after a 2-second delay. Except for the network address of the audio, all other parameters can be omitted.
playNetAudio("http://example.com/a.mp3", {x: '540', y: '350', duration: 3000, delay: 2000})
playNetAudio("http://example.com/a.mp3", {x: '50%', y: '50%', delay: 2000})
playNetAudio("http://example.com/a.mp3")

// Play ringtone.
playRingtone()

```

### Features supported starting from version 4.6.7

```js
4.6.7 features supported starting from this version

// Simplest call: directly pass a text string.
let result = input("你好世界");

// Object parameter: only pass text.
result = input({ text: "Hello World" });

// Use clipboard paste mode for input (useClipboard: true).
result = input({ text: "Batch content", useClipboard: true });

// Use Wubi input method to type (useWubi: true).
result = input({ text: "Chinese text content", useWubi: true });

// In PC mode, do not use PC input (noPcInput: true).
result = input({ text: "Content", noPcInput: true });

// inputType specifies appending content after the original content in the input box.
result = input({ text: "Repeated content", inputType: 1 });

// Combined parameters: Wubi + no PC input + append content after the original content in the input box.
result = input({ text: "Test text", useWubi: true, noPcInput: true, inputType: 1 });

// Use clipboard.
result = input({ text: "Quick input", useClipboard: true });

// Parse return value (input returns a JSON string).
let resultJson = input({ text: "Test" });
let resultObj = JSON.parse(resultJson);

```

### Features supported starting from version 4.6.7

```js
4.6.7 features supported starting from this version

// Basic: only pass image path/base64.
let result = imgMatch({ text: "/sdcard/target.png" });

// Specify the search area (left/top/right/bottom ratio or pixels).
result = imgMatch({
    text: "/sdcard/target.png",
    left: 0,
    top: 0,
    right: 1080,
    bottom: 600
});


// Specify similarity (sim, 0~100). Default is 90.
result = imgMatch({
    text: "/sdcard/icon.png",
    sim: 90
});

// Specify image preprocessing type. 0 means no processing by default.
result = imgMatch({
    text: "/sdcard/icon.png",
    imagePreType: 0
});

// Specify color filtering (perargb array).
result = imgMatch({
    text: "/sdcard/icon.png",
    imagePreType: 513,
    perargb: [255, 0, 0, 255]  // [a, r, g, b]
});

// Multi-image matching (pass an array for text).
result = imgMatch({
    text: ["/sdcard/img1.png", "/sdcard/img2.png"],
    sim: 85
});

// Full parameter combination.
result = imgMatch({
    text: "/sdcard/target.png",
    left: 100,
    top: 200,
    right: 900,
    bottom: 1600,
    sim: 95,
    imagePreType: 513,
    perargb: [255, 255, 0, 0]
});
var location = JSON.parse(result);// Format as JSON.
for (var i=0;i<location.length;i++)
{ 
    log('Image center x coordinate (pixels)'+ location[i].x);
    log('Image center y coordinate (pixels)'+ location[i].y);
    log('Image left coordinate (pixels)'+ location[i].left);
    log('Image top coordinate (pixels)'+ location[i].top);
    log('Image right coordinate (pixels)'+ location[i].right);
    log('Image bottom coordinate (pixels)'+ location[i].bottom);
}
Optional configuration values for imagePreType are:
401,"X-axis edge detection processing",
402,"Y-axis edge detection processing",
403,"XY-axis edge detection processing",
404,"Edge detection from upper-left to lower-right",
405,"Edge detection from upper-right to lower-left",
406,"Low-threshold Canny edge detection",
407,"High-threshold Canny edge detection",
408,"Canny edge detection after filtering",
409,"Laplacian edge detection",
410,"Laplacian edge detection after Gaussian filtering",
411,"Scharr operator X edge detection",
412,"Scharr operator Y edge detection",
413,"Scharr operator XY edge detection",
414,"boxFilter normalized",
415,"boxFilter not normalized",
416,"sqrBoxFilter normalized",
417,"Desaturate RGB2GRAY",
418,"Desaturate DeColor",
419,"Desaturate ColorBoosting",
420,"Add 70% transparent black mask to screenshot",
421,"Add 70% transparent black mask to target image",
501,"Extract black",
502,"Extract gray",
503,"Extract white",
504,"Extract red",
505,"Extract orange",
506,"Extract yellow",
507,"Extract green",
508,"Extract cyan",
509,"Extract blue",
512,"Extract custom color",
510,"Extract purple",
513,"HSV color value adjustment",

```

### Features supported starting from version 4.6.7

```js
// Jump to step. After jumping away, it means the JS code after it will not continue to execute.
gotoStep(3); // Jump to step 3 (steps start from 1).

// Pause the flow (popup; after the user clicks OK, execution continues with the following code).
pauseFlow(); // Use the default prompt.
pauseFlow("Please confirm the operation before continuing"); // Custom prompt.

// Continue executing the following code after pausing.
log("Start execution");
pauseFlow("Check it. If everything is OK, click Confirm");
log("The user clicked Confirm; continue execution"); // This is executed only after the user clicks OK.

// Stop the current flow.
stopFlow();
// Code after stopFlow() will not execute.

// Stop all flows.
stopAllFlow();
// Code after stopAllFlow() will not execute.


// Normal word library (word libraries used in JS code will not be automatically included during cloud backup and packaging, so you need to use this word library in the variable control of a step so it will be included automatically).
var json = getWordLibrary("My word library title");
log("Raw data of normal word library: " + json);

var words = JSON.parse(json);
log("Normal word library has " + words.length + " entries");
for (var i = 0; i < words.length; i++) {
    log("Entry " + (i + 1) + ": " + words[i]);
}


// Keyword word library (word libraries used in JS code will not be automatically included during cloud backup and packaging, so you need to use this word library in the variable control of a step so it will be included automatically).
var json2 = getKeyWordLibrary("My keyword word library title");
log("Raw data of keyword library: " + json2);

var items = JSON.parse(json2);
log("Keyword word library has " + items.length + " groups");
for (var i = 0; i < items.length; i++) {
    var item = items[i];
    log("Group " + (i + 1) + " keywords: " + JSON.stringify(item.keys));
    log("Group " + (i + 1) + " contents: " + JSON.stringify(item.contents));
}


// Screen screenshot (in-memory screenshot). Remember to release it with recycle(imgId) after use; otherwise it consumes memory. If it is not used within 30 seconds after screenshotting, it will also be released automatically.
let imgId = captureScreen();
if (imgId == null) {
    log("Screenshot failed");
    return;
}
log("Screenshot succeeded, image ID=" + imgId);

// Extract the color in an image through the imgId of the screenshot.
try {
    let colorJson = pixel(imgId, 100, 200);
    let col = JSON.parse(colorJson);
  
    log('Hex color value: ' + col.hexColor);      // #FF0000
    log('Color value: ' + col.color);                 // -8217131
    log('Unsigned color value: ' + col.unsignedColor);   // 4286750105
    log('RGB: R=' + col.r + ', G=' + col.g + ', B=' + col.b);  // RGB: R=255, G=0, B=0
    log('Alpha: ' + col.a);                      // 255
} finally {
// Release the memory occupied by the image.
    recycle(imgId);
}


// Get image width and height.
let width = getImgWidth(imgId);
log('Width: ' + width + 'px');  // Width: 1080px
let height = getImgHeight(imgId);
log('Height: ' + height + 'px');  // Height: 1920px
```

　

### Features supported starting from version 4.6.9

```js
// YOLO object detection.
// ========== Method 1: Automatic screenshot detection ==========
var result = yoloRunModel({
  model: 'your model name',   // Model name in the model management page.
  left: '10%', top: '20%', right: '90%', bottom: '80%',  // Limit recognition area. Pixels/dp/percentages are supported.
  score: 50               // Matching score percentage. Default is 90 if not passed.
});

log('YOLO detection result: ' + result);

if (result == null) {
  log('Detection failed or target not found');
} else {
  var items = JSON.parse(result);
  log('Number of targets detected: ' + items.length);
  for (var i = 0; i < items.length; i++) {
    log('--- Target ' + (i + 1) + ' ---');
    log('Label: ' + items[i].label);
    log('Similarity: ' + items[i].score + '%');
    log('Center x coordinate: ' + items[i].centerX);
    log('Center y coordinate: ' + items[i].centerY);
    log('Left: ' + items[i].left);
    log('Top: ' + items[i].top);
    log('Right: ' + items[i].right);
    log('Bottom: ' + items[i].bottom);
  }
}


// ========== Method 2: Reuse an existing screenshot (more efficient when taking one screenshot for multiple detections) ==========
var imgId = captureScreen();  // Take a screenshot first.
log('Screenshot ID: ' + imgId);

if (imgId == null) {
  log('Screenshot failed');
} else {
  var result2 = yoloRunModel({
    model: 'your model name',
    score: 50,
    imgId: imgId   // Pass in screenshot ID and skip automatic screenshotting.
  });

  // Release immediately after use to prevent memory leaks.
  recycle(imgId);

  log('YOLO detection result: ' + result2);

  if (result2 == null) {
    log('Detection failed or target not found');
  } else {
    var items2 = JSON.parse(result2);
    log('Number of targets detected: ' + items2.length);
    for (var i = 0; i < items2.length; i++) {
      log('--- Target ' + (i + 1) + ' ---');
      log('Label: ' + items2[i].label);
      log('Similarity: ' + items2[i].score + '%');
      log('Center x: ' + items2[i].centerX + '  Center y: ' + items2[i].centerY);
      log('Area: left=' + items2[i].left + ' top=' + items2[i].top
          + ' right=' + items2[i].right + ' bottom=' + items2[i].bottom);
    }
  }
}



// ========== readExcel / writeExcel read and write Excel ==========

// ========== Read local Excel ==========
var result = readExcel(JSON.stringify({
  filePath: '/sdcard/客户列表.xlsx',  // Full path of the Excel file.
  sheetName: 'Sheet1',               // Sheet name. If not filled, the first Sheet is read.
  row: 2,                            // Row number, starting from 1.
  cols: [1, 2, 3]                    // Column numbers to read, starting from 1.
}));

var r = JSON.parse(result);
log('Read result: ' + result);

if (!r.success) {
  log('Read failed: ' + r.msg);
} else {
  log('Read row number: ' + r.row);
  log('Column 1 value: ' + r.values[0]);
  log('Column 2 value: ' + r.values[1]);
  log('Column 3 value: ' + r.values[2]);
}


// ========== Write local Excel (overwrite specified row) ==========
var result2 = writeExcel(JSON.stringify({
  filePath: '/sdcard/结果.xlsx',   // Full path of the Excel file.
  sheetName: 'Sheet1',            // Sheet name. If not filled, the first Sheet is written.
  row: 3,                         // Row number, starting from 1.
  cells: [
    {col: 1, value: 'Zhang San'},      // col column number starts from 1; value is the content to write.
    {col: 2, value: 'Completed'},
    {col: 3, value: '2024-01-01'}
  ]
}));

var w = JSON.parse(result2);
log('Write result: ' + result2);

if (!w.success) {
  log('Write failed: ' + w.msg);
} else {
  log('Write succeeded, row number: ' + w.row);
}


// ========== Write local Excel (row=-1 appends to the end) ==========
var result3 = writeExcel(JSON.stringify({
  filePath: '/sdcard/结果.xlsx',
  row: -1,                         // -1 means append after the last row.
  cells: [
    {col: 1, value: 'Li Si'},
    {col: 2, value: 'Processing'}
  ]
}));

var w2 = JSON.parse(result3);
log('Append result: ' + result3);

if (!w2.success) {
  log('Append failed: ' + w2.msg);
} else {
  log('Append succeeded, row number: ' + w2.row);
}


// ========== Read remote Excel (Mingdao Cloud); blocks while waiting for return ==========
var result4 = readExcel(JSON.stringify({
  remote: true,                    // true means reading remote data.
  worksheetId: 'your worksheetId',  // Mingdao Cloud worksheet ID.
  appKey: 'your appKey',            // Mingdao Cloud AppKey.
  sign: 'your sign',                // Mingdao Cloud Sign.
  viewId: 'your viewId',           // View ID; optional.
  row: 1,                          // Row number starts from 1; -1 means reading the last row.
  fieldKeys: ['Name', 'Phone Number', 'Status']  // Array of field names to read.
}));

var r2 = JSON.parse(result4);
log('Remote read result: ' + result4);

if (!r2.success) {
  log('Remote read failed: ' + r2.msg);
} else {
  log('Read row number: ' + r2.row);
  log('Row ID: ' + r2.rowId);          // The unique ID of this row in Mingdao Cloud; it can be used for positioning during updates.
  log('Name: ' + r2.values[0]);
  log('Phone number: ' + r2.values[1]);
  log('Status: ' + r2.values[2]);
}


// ========== Write remote Excel (row=-1 appends a new row) ==========
var result5 = writeExcel(JSON.stringify({
  remote: true,
  worksheetId: 'your worksheetId',
  appKey: 'your appKey',
  sign: 'your sign',
  row: -1,                          // -1 means appending a new row.
  fields: [
    {fieldKey: 'Name',   value: 'Wang Wu'},   // fieldKey field name must match Mingdao Cloud.
    {fieldKey: 'Phone Number', value: '[电话]'},
    {fieldKey: 'Status',   value: 'Pending'}
  ]
}));

var w3 = JSON.parse(result5);
log('Remote append result: ' + result5);

if (!w3.success) {
  log('Remote append failed: ' + w3.msg);
} else {
  log('Append succeeded, row ID: ' + w3.rowId);
  log('Whether a new row was appended: ' + w3.append);   // true = appended a new row, false = updated an existing row.
}


// ========== Write remote Excel (update specified row) ==========
var result6 = writeExcel(JSON.stringify({
  remote: true,
  worksheetId: 'your worksheetId',
  appKey: 'your appKey',
  sign: 'your sign',
  row: 2,                           // Update row 2.
  fields: [
    {fieldKey: 'Status', value: 'Completed'}
  ]
}));

var w4 = JSON.parse(result6);
log('Remote update result: ' + result6);

if (!w4.success) {
  log('Remote update failed: ' + w4.msg);
} else {
  log('Update succeeded, row number: ' + w4.row + ', row ID: ' + w4.rowId);
}


// ========== Comprehensive example: read local Excel row by row and write to remote Mingdao Cloud ==========
var rowNum = 2;  // Start from row 2. Row 1 is usually the header.

while (true) {
  var readResult = readExcel(JSON.stringify({
    filePath: '/sdcard/客户列表.xlsx',
    row: rowNum,
    cols: [1, 2, 3]
  }));

  var rd = JSON.parse(readResult);

  if (!rd.success) {
    log('Row ' + rowNum + ' read ended or failed: ' + rd.msg);
    break;
  }

  log('Read row ' + rowNum + ': ' + rd.values[0] + ' / ' + rd.values[1] + ' / ' + rd.values[2]);

  var writeResult = writeExcel(JSON.stringify({
    remote: true,
    worksheetId: 'your worksheetId',
    appKey: 'your appKey',
    sign: 'your sign',
    row: -1,
    fields: [
      {fieldKey: 'Name',   value: rd.values[0]},
      {fieldKey: 'Phone Number', value: rd.values[1]},
      {fieldKey: 'Notes',   value: rd.values[2]}
    ]
  }));

  var wd = JSON.parse(writeResult);

  if (!wd.success) {
    log('Row ' + rowNum + ' upload failed: ' + wd.msg);
  } else {
    log('Row ' + rowNum + ' uploaded successfully, row ID: ' + wd.rowId);
  }

  rowNum++;
  sleep(500);
}
```

```

```

　

### More features are under development