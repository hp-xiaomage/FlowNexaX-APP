# 💯 当前教程对应的是3.8.1及之后的版本💯

3.8.1版本开始，FlowNexaX升级了引擎，支持更多常规的语法。

# 运行JS代码

通过编写 JS 代码，实现最灵活的脚本逻辑控制和增强的扩展能力

> 编写 JS 代码需要有 JS 代码语言基础，可以从网上搜索学习。如：
>
> * JavaScript 语言入门教程: [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide](https://wangdoc.com/javascript/)
> * JavaScript 教程: [https://javascript.info/](https://www.runoob.com/js/js-tutorial.html)

除了 JS 引擎通用能力外，FlowNexaX内还扩展了以下能力：

### 建议

```js

手机端写JS代码相对麻烦，建议用电脑写完后复制到FlowNexaX中，长按app内的js编辑框，有PASTE黏贴按键，也有其他的按键，比如撤销、重写、剪切、复制、全选；

当然了，对于编写代码来讲，还是不适合用手机来写，请注意，在app内编写代码时，用中文输入法会存在输入时会先删除前一个字符，这是输入法的问题，还是建议你用电脑写完发给手机。

app中的编写框会帮你检测JS语法的正确与否，在左侧的行数有警告的图标。

```

### 通用 API

```js

// 获取当前FlowNexaXApp 版本号，如“3.4.0”
var version=getAppVersion();
log(version);

//设置当前系统剪切板文本
var result=setClipboard('三生三世');
log(result);

//获取当前系统剪切板文本
var content=getClipboard();
log(content);

// 显示一个自动消失的简易提示
toast('提示'); 

// 等待 1s
sleep(1000); 

```

### 全局及系统变量

```js

// 读取系统变量或全局变量内的变量 a
var variable=getVar('系统-坐1');
log(variable);

// 写入变量 系统-偷卡 的值为字符 "aaa"
setVar('系统-偷卡', 'aaa');

// 3.9.6版本开始系统变量支持设置是否持久化保存 写入变量 系统-偷卡 的值为字符 "aaa"，true代表要持久保持
setVar('系统-偷卡', 'aaa', true);

// 读取全局变量内的所有变量
var vars=getVars();
log(vars);//日志打印所有变量json串
var varsObj= JSON.parse(vars);//变量JSON格式化
var content=varsObj['坐标a']['vcontent'];//提取变量 坐标a 的内容


// 读取系统变量内的所有变量
var sysVars=getSysVars();
log(sysVars);//日志打印所有变量json串
var sysVarsObj= JSON.parse(sysVars);//变量JSON格式化
var content=sysVarsObj['系统-坐1']['vcontent'];//提取变量 系统-坐1 的内容

// 455 版本支持添加系统变量addSysVar
addSysVar({
  type: 1, // 1代表字符，2代表数字，3代表坐标单个，4代表坐标多个，5代表图片，6代表时间
  name: '系统-我的变量名字', // 变量名
  group: '我的分组', //变量分组名
  defValue: '这是我的默认值', //默认值
});


```

### 打印日志

用于在运行日志面板上输出你要的信息。

```js

log("日志信息"); // 输出日志到运行日志面板

clearLog(); // 清除运行日志

```

### 步骤动作

跟你添加或录制的步骤一样

```js

// 点击步骤

click(x, y, duration); // 点击 x,y 的坐标位置，点击时长 duration 毫秒

click('50%', '50%'); // 点击屏幕中间

click('200dp', '100dp'); // 点击屏幕 200dp, 100dp 逻辑像素位置

click(300, 500, 1000); // 长按屏幕像素 300,500 的位置 1000毫秒(1秒)

longClick('50%', '50%'); // 长按屏幕中间位置，同 ckjl.click('50%', '50%', 600)


// 滑动步骤：

swipe(x1, y1, x2, y2, duration); // 从 x1,y1 滑动到 x2,y2，滑动时长 duration 毫秒

swipe('50%', '50%', '50%', '10%', 1000); // 从屏幕中间向上滑动，滑动时长1000毫秒(50%,50% -> 50%,10%)


// 单指手势：

gesture(duration, [x1, y1], [x2, y2], ...); // 手势滑动，省略号的意思是可以无限加

gesture(2000, ['50%', '10%'], ['50%', '50%'], ['80%', '50%']); // 手势滑动(50%,10% -> 50%,50% -> 80%,50%)，时长 2000毫秒(2秒)，也支持像素或者dp


// 多指操作：

gestures([delay1, duration1, [x1, y1], [x2, y2], ...], [delay2, duration2, [x3, y3], [x4, y4], ...], ...); // 多指手势。每个手势的参数为[delay, duration, 坐标1, 坐标2, ...], delay为延迟多久(毫秒)才执行该手势；duration为手势执行时长(毫秒)；坐标为手势经过的点的坐标。

gestures([0, 2000, ['80%', '20%'], ['50%', '50%']],
              [0, 2000, ['20%', '80%'], ['50%', '50%']]); // 双指捏合手势，也支持像素或者dp

```

支持的坐标单位

* 物理像素坐标，如：(100, 100)。
* 逻辑像素坐标，如：('200dp', '100dp')。
* 屏幕百分比坐标，如：('50%', '30%')。推荐使用百分比坐标保证多屏幕适配性

### 查找坐标

可以根据传入的条件，查找屏幕上的坐标。（屏幕左上角为坐标轴原点，x轴从左向右，y轴从上向下）

```js
// 查找屏幕上一处文字坐标
var result= findLocation({
  type: 'nodetext', // 控件文字类型
  text: '文本', // 文字内容,正则表达式也支持，比如写成  正则@[\\d+]  就是代表匹配数字
  left: '20%', //限制查找区域左边位置 支持像素、dp、百分比
  top: '20dp', //限制查找区域上边位置 支持像素、dp、百分比
  right: '800', //限制查找区域右边位置 支持像素、dp、百分比
  bottom: '60%', //限制查找区域下边位置 支持像素、dp、百分比
  like: true,  //填true代表模糊匹配；false代表完全匹配
  sim: 100,  //文字的相似度百分比，填100代表文字必须一字不差
});
var location = JSON.parse(result);//json格式化
log('文字中心x坐标(像素)'+ location);
log('文字中心x坐标(像素)'+ location.x);
log('文字中心y坐标(像素)'+ location.y);
log('文字左边坐标(像素)'+ location.left);
log('文字上边坐标(像素)'+ location.top);
log('文字右边坐标(像素)'+ location.right);
log('文字下边坐标(像素)'+ location.bottom);




// 查找屏幕上多处文字坐标
var result = findLocation({
  type: 'nodetext', // 控件文字类型
  text: '8', // 文字内容,正则表达式也支持，比如写成  正则@[\\d+]  就是代表匹配数字
  left: '20%', //限制查找区域左边位置 支持像素、dp、百分比
  top: '20dp', //限制查找区域上边位置 支持像素、dp、百分比
  right: '800', //限制查找区域右边位置 支持像素、dp、百分比
  bottom: '60%', //限制查找区域下边位置 支持像素、dp、百分比
  like: true,  //填true代表模糊匹配；false代表完全匹配
  sim: 0,  //文字的相似度百分比，填0代表忽略这个配置
},true);  //这里的true代表查找所有位置
var location = JSON.parse(result);//json格式化
for (var i=0;i<location.length;i++)
{ 
    log('文字中心x坐标(像素)'+ location[i].x);
    log('文字中心y坐标(像素)'+ location[i].y);
    log('文字左边坐标(像素)'+ location[i].left);
    log('文字上边坐标(像素)'+ location[i].top);
    log('文字右边坐标(像素)'+ location[i].right);
    log('文字下边坐标(像素)'+ location[i].bottom);
}




// 查找屏幕上一处文本坐标（图文识别）
var result = findLocation({
  type: 'ocrtext', // 图文识别类型
  text: '6', // 文字内容,正则表达式也支持，比如写成  正则@[\\d+]  就是代表匹配数字
  left: '20%', //限制查找区域左边位置 支持像素、dp、百分比
  top: '20dp', //限制查找区域上边位置 支持像素、dp、百分比
  right: '1000', //限制查找区域右边位置 支持像素、dp、百分比
  bottom: '90%', //限制查找区域下边位置 支持像素、dp、百分比
  like: true,  //填true代表模糊匹配；false代表完全匹配
  pre: '511', //使用原图
  sim: 80,  //文字的相似度百分比，填80代表识别到的文字内容必须达到百分之80的相似度
});
var location = JSON.parse(result);
log('文本中心x坐标(像素)'+ location.x);
log('文本中心y坐标(像素)'+ location.y);
log('文本左边坐标(像素)'+ location.left);
log('文本上边坐标(像素)'+ location.top);
log('文本右边坐标(像素)'+ location.right);
log('文本下边坐标(像素)'+ location.bottom);




// 查找屏幕上多处文本坐标（图文识别）
var result = findLocation({
  type: 'ocrtext', // 图文识别类型
  text: '文本', // 文字内容,正则表达式也支持，比如写成  正则@[\\d+]  就是代表匹配数字
  left: '20%', //限制查找区域左边位置 支持像素、dp、百分比
  top: '20dp', //限制查找区域上边位置 支持像素、dp、百分比
  right: '800', //限制查找区域右边位置 支持像素、dp、百分比
  bottom: '60%', //限制查找区域下边位置 支持像素、dp、百分比
  like: true,  //填true代表模糊匹配；false代表完全匹配
  pre: '504', //提取红色
  sim: 80,  //文字的相似度百分比，填80代表识别到的文字内容必须达到百分之80的相似度
},true);//这里的true代表查找所有位置
var location = JSON.parse(result);//json格式化
for (var i=0;i<location.length;i++)
{ 
    log('文本中心x坐标(像素)'+ location[i].x);
    log('文本中心y坐标(像素)'+ location[i].y);
    log('文本左边坐标(像素)'+ location[i].left);
    log('文本上边坐标(像素)'+ location[i].top);
    log('文本右边坐标(像素)'+ location[i].right);
    log('文本下边坐标(像素)'+ location[i].bottom);
}

// 查找屏幕上多处文本坐标（图文识别）
var result = findLocation({
  type: 'ocrtext', // 图文识别类型
  text: '文本', // 文字内容,正则表达式也支持，比如写成  正则@[\\d+]  就是代表匹配数字
  left: '20%', //限制查找区域左边位置 支持像素、dp、百分比
  top: '20dp', //限制查找区域上边位置 支持像素、dp、百分比
  right: '800', //限制查找区域右边位置 支持像素、dp、百分比
  bottom: '60%', //限制查找区域下边位置 支持像素、dp、百分比
  like: true,  //填true代表模糊匹配；false代表完全匹配
  pre: '512', //提取自定义颜色，自定义颜色要438版本才支持
  r: 3, //提取自定义颜色必填，rgb就是颜色的值，具体百度RGB颜色值
  g: 202, //提取自定义颜色必填，rgb就是颜色的值，具体百度RGB颜色值
  b: 99, //提取自定义颜色必填，rgb就是颜色的值，具体百度RGB颜色值
  sim: 80,  //文字的相似度百分比，填80代表识别到的文字内容必须达到百分之80的相似度
},true);//这里的true代表查找所有位置
var location = JSON.parse(result);//json格式化
for (var i=0;i<location.length;i++)
{ 
    log('文本中心x坐标(像素)'+ location[i].x);
    log('文本中心y坐标(像素)'+ location[i].y);
    log('文本左边坐标(像素)'+ location[i].left);
    log('文本上边坐标(像素)'+ location[i].top);
    log('文本右边坐标(像素)'+ location[i].right);
    log('文本下边坐标(像素)'+ location[i].bottom);
}

以上pre可配置的提取色有：
GET_BLACK(501,"提取黑色"),
GET_GRAY(502,"提取灰色"),
GET_WHITE(503,"提取白色"),
GET_RED(504,"提取红色"),
GET_ORANGE(505,"提取橙色"),
GET_YELLOW(506,"提取黄色"),
GET_GREEN(507,"提取绿色"),
GET_CYAN(508,"提取青色"),
GET_BLUE(509,"提取蓝色"),
GET_PURPLE(510,"提取紫色"),
GET_YUANTU(511,"使用原图"),
GET_COUSTOM(512,"提取自定义颜色"),





// 查找屏幕上多处控件坐标
var result = findLocation({
  type: 'node', // 控件类型
  className: 'android.widget.TextView',  //控件类名
  nodeText: '摄影摄像',  //控件文字,正则表达式也支持，比如写成  正则@[\\d+]  就是代表匹配数字
  nodeDes: '描述',  //控件描述
  nodeId: 'icon_title',  //控件id
  packageName: 'com.tengxun',  //控件包名
  scrollable: true,  //控件是否可滚动 true为是，false为否
  clickable: false,  //控件是否可点击
  longClickable: true,  //控件是否可长按
  left: '20%', //限制查找区域左边位置 支持像素、dp、百分比
  top: '20dp', //限制查找区域上边位置 支持像素、dp、百分比
  right: '800', //限制查找区域右边位置 支持像素、dp、百分比
  bottom: '60%', //限制查找区域下边位置 支持像素、dp、百分比
},true);  //这里的true代表查找所有位置，false代表只要查一处
var location = JSON.parse(result);
for (var i=0;i<location.length;i++)
{ 
    log('控件中心x坐标(像素)', location[i].x);
    log('控件中心y坐标(像素)', location[i].y);
    log('控件左边坐标(像素)', location[i].left);
    log('控件上边坐标(像素)', location[i].top);
    log('控件右边坐标(像素)', location[i].right);
    log('控件下边坐标(像素)', location[i].bottom);
}

```

### 查找控件

可以根据传入的条件，查找屏幕上的控件。

```js
// 查找屏幕上一处控件
var result = findNode({
  className: 'android.widget.TextView',  //控件类名
  nodeText: '摄影摄像',  //控件文字,正则表达式也支持，比如写成  正则@[\\d+]  就是代表匹配数字
  nodeDes: '描述',  //控件描述
  nodeId: 'icon_title',  //控件id
  packageName: 'com.tengxun',  //控件包名
  scrollable: true,  //控件是否可滚动 true为是，false为否
  clickable: false,  //控件是否可点击
  longClickable: true,  //控件是否可长按
});
var node=JSON.parse(result);
log('控件文字'+node.nodeText);
log('控件类名'+ node.className);
log('控件包名'+ node.packageName);
log('控件id'+ node.nodeId);
log('控件描述'+ node.nodeDes);
log('控件是否可长按'+ node.isLongClickable);
log('控件是否可滚动'+ node.isScrollable);
log('控件是否可点击'+ node.isClickable);
log('控件左边框距屏幕左侧距离'+node.left);
log('控件上边框距屏幕顶部距离'+ node.top);
log('控件右边框距屏幕左侧距离'+ node.right);
log('控件下边框距屏幕顶部距离'+node.bottom);



// 查找屏幕上多处控件
var result = findNode({
  className: 'android.widget.TextView',  //控件类名
  nodeText: '摄影摄像',  //控件文字,正则表达式也支持，比如写成  正则@[\\d+]  就是代表匹配数字
  nodeDes: '描述',  //控件描述
  nodeId: 'icon_title',  //控件id
  packageName: 'com.tengxun',  //控件包名
  scrollable: true,  //控件是否可滚动 true为是，false为否
  clickable: false,  //控件是否可点击
  longClickable: true,  //控件是否可长按
},true);// true代表查处符合的所有控件
var node=JSON.parse(result);
for (var i=0;i<node.length;i++)
{ 
  log('-------------------------'+i);
  log('控件文字'+node[i].nodeText);
  log('控件类名'+ node[i].className);
  log('控件包名'+ node[i].packageName);
  log('控件左边框距屏幕左侧距离'+node[i].left);
  log('控件上边框距屏幕顶部距离'+ node[i].top);
  log('控件右边框距屏幕左侧距离'+ node[i].right);
  log('控件下边框距屏幕顶部距离'+node[i].bottom);
}




// 查找屏幕上多处控件及其子控件
var result = findNode({
  className: 'android.widget.RelativeLayout',
  left: '20%', //限制查找区域左边位置 支持像素、dp、百分比
  top: '20dp', //限制查找区域上边位置 支持像素、dp、百分比
  right: '800', //限制查找区域右边位置 支持像素、dp、百分比
  bottom: '60%', //限制查找区域下边位置 支持像素、dp、百分比
},true,true);//第一个true代表查处多个控件，第二个true代表查处控件的子控件
var node=JSON.parse(result);
for (var i=0;i<node.length;i++)
{ 
  log('-------------------------'+i);

  log('控件类名'+ node[i].className);
  if(node[i].children){
     var childlength=node[i].children.length;
     log('子控件数量'+ childlength);
    for (var y=0;y<childlength;y++)
    { 
       var childlist=node[i].children[y];
       log('子控件类名'+ childlist.className);
    } 

  }

}



```

### 屏幕识别

可以识别屏幕指定位置的 控件文字、或识别图片里面的文字。

```js

// 识别屏幕上的文字控件提取文字
var result = getScreenText({
  type: 'nodetext',  //识别文字控件
  left: '20%', //限制查找区域左边位置 支持像素、dp、百分比
  top: '20dp', //限制查找区域上边位置 支持像素、dp、百分比
  right: '800', //限制查找区域右边位置 支持像素、dp、百分比
  bottom: '60%', //限制查找区域下边位置 支持像素、dp、百分比
});
var texts=JSON.parse(result);
for (var i=0;i<texts.length;i++)
{ 
  log('-------------------------'+i);
  log('文字内容：'+ texts[i].text);
  log('文字位置：左边：'+ texts[i].left);
  log('文字位置：上边：'+ texts[i].top);
  log('文字位置：右边：'+ texts[i].right);
  log('文字位置：下边：'+ texts[i].bottom);

}




// 识别屏幕上的文本（图文识别）
var result = getScreenText({
  type: 'ocrtext',  //识别文字控件
  left: '20%', //限制查找区域左边位置 支持像素、dp、百分比
  top: '20dp', //限制查找区域上边位置 支持像素、dp、百分比
  right: '800', //限制查找区域右边位置 支持像素、dp、百分比
  bottom: '60%', //限制查找区域下边位置 支持像素、dp、百分比
  pre: '511', //提取红色
});
var texts=JSON.parse(result);
for (var i=0;i<texts.length;i++)
{ 
  log('-------------------------'+i);
  log('文字内容：'+ texts[i].text);
  log('文字位置：左边：'+ texts[i].left);
  log('文字位置：上边：'+ texts[i].top);
  log('文字位置：右边：'+ texts[i].right);
  log('文字位置：下边：'+ texts[i].bottom);

}

// 识别屏幕上的文本（图文识别）
var result = getScreenText({
  type: 'ocrtext',  //识别文字控件
  left: '20%', //限制查找区域左边位置 支持像素、dp、百分比
  top: '20dp', //限制查找区域上边位置 支持像素、dp、百分比
  right: '800', //限制查找区域右边位置 支持像素、dp、百分比
  bottom: '60%', //限制查找区域下边位置 支持像素、dp、百分比
  pre: '512', //提取自定义颜色，自定义颜色要438版本才支持
  r: 3, //提取自定义颜色必填，rgb就是颜色的值，具体百度RGB颜色值
  g: 202, //提取自定义颜色必填，rgb就是颜色的值，具体百度RGB颜色值
  b: 99, //提取自定义颜色必填，rgb就是颜色的值，具体百度RGB颜色值
});
var texts=JSON.parse(result);
for (var i=0;i<texts.length;i++)
{ 
  log('-------------------------'+i);
  log('文字内容：'+ texts[i].text);
  log('文字位置：左边：'+ texts[i].left);
  log('文字位置：上边：'+ texts[i].top);
  log('文字位置：右边：'+ texts[i].right);
  log('文字位置：下边：'+ texts[i].bottom);

}

以上pre可配置的提取色有：
GET_BLACK(501,"提取黑色"),
GET_GRAY(502,"提取灰色"),
GET_WHITE(503,"提取白色"),
GET_RED(504,"提取红色"),
GET_ORANGE(505,"提取橙色"),
GET_YELLOW(506,"提取黄色"),
GET_GREEN(507,"提取绿色"),
GET_CYAN(508,"提取青色"),
GET_BLUE(509,"提取蓝色"),
GET_PURPLE(510,"提取紫色"),
GET_YUANTU(511,"使用原图");
GET_YUANTU(512,"提取自定义颜色");



// 提取指定坐标位置的颜色值
var result = getScreenColor('20%','20dp'); //查找位置 支持像素、dp、百分比
var col=JSON.parse(result);
log('十六进制颜色值内容：'+ col.hexColor);
log('颜色值内容：'+ col.color);
log('颜色值中的Alpha通道清零后的颜色值内容：'+ col.unsignedColor);



// 发送请求使用示例
// 构造请求体对象
var requestBodyObj = {
    "user_id": "你的账号",
    "messages": [
        {
            "content": "消息",
            "content_type": "text",
            "role": "user",
            "type": "question"
        }
    ]
};
var headerParamObj = {
        'Authorization': "Bearer 你的token",
        'Content-Type': 'application/json'
    };

var configParamObj = {
        connectTimeout: 60,
        readTimeout: 60,
        writeTimeout: 60
    } ;

// 动序列化为JSON字符串
var requestBodyStr = JSON.stringify(requestBodyObj);
var headerParamStr = JSON.stringify(headerParamObj);
var configParamStr = JSON.stringify(configParamObj);

// 发送请求 
var result = requestUrl({ 
    url: 'http://www.baidu.com', 
    method: 'POST', 
    // 直接传序列化后的字符串
    inputParam: requestBodyStr, //请求体body入参
    headerParam: headerParamStr, //请求头入参
    configParam: configParamStr  //请求配置入参，例如超时时间
}); 
var content=JSON.parse(result);
log('请求返回的内容：'+ content);
```

### 3.8.5版本开始支持的功能：

### url scheme跳转目标页面

可以打开目标APP指定的页面。

```js

// 使用 URL scheme 打开目标 APP 的指定页面。例如，可以打开国际常用 APP 中的聊天、主页、视频、地图、邮件或拨号页面，前提是目标 APP 支持对应的 URL scheme 或意图行为。
startActivity({
     data: "目标 APP URL scheme",
});

// 示例：打开 WhatsApp 聊天。
startActivity({
     data: "whatsapp://send?phone=15551234567",
});

// 示例：打开 Telegram 用户或频道。
startActivity({
     data: "tg://resolve?domain=telegram",
});

// 示例：打开 Instagram 主页。
startActivity({
     data: "instagram://user?username=instagram",
});

// 示例：打开 YouTube 页面。
startActivity({
     data: "youtube://www.youtube.com/@YouTube",
});

// 示例：在地图 APP 中打开位置。
startActivity({
     data: "geo:0,0?q=Times+Square+New+York",
});

// 示例：打开邮件 APP。
startActivity({
     data: "mailto:support@example.com?subject=Hello",
});

// 示例：打开拨号页面。
startActivity({
     data: "tel:+15551234567",
});
```

　

### 弹框功能

可以弹出一个确定框。

```js

alert("弹框提示的内容");//默认同步弹框，会等待点击确认后才继续往下运行

alert("弹框提示的内容",false);//false代表异步弹出，不会阻碍后面代码的运行


confirm("弹框提示的内容");//带有确定、取消按钮的弹框，默认同步弹框

confirm("弹框提示的内容",false);//带有确定、取消按钮的弹框，false代表异步弹框


var result=confirm("弹框提示的内容","同意","不同意"); //自定义按钮的描述弹框，默认同步弹框，result会返回点击的是哪个按钮
log(result);

confirm("弹框提示的内容","同意","不同意",false);//自定义按钮的描述弹框，false代表异步弹框，result会返回点击的是哪个按钮


```

　

　

### 4.0.2版本开始支持的功能：

### 执行ＡＤＢ命令，开了ＲＯＯＴ的手机

可以执行ｒｏｏｔ命令。

```js

runRoot("你的root命令");

例如：
//从屏幕的xy坐标100,100滑动到屏幕的xy坐标400,400，滑动耗时1秒。
runRoot("input swipe 100 100 400 400 1000");

//单击屏幕的xy坐标100,100，点击耗时1秒。
runRoot("input swipe 100 100 100 100 1000");

//双击屏幕的xy坐标100,100，双击间隔0.14秒。
runRoot("seq 2 | while read i;do input tap 100 100 & input tap 100 100 & sleep 0.14;done");

//点击返回键。
runRoot("input keyevent 4");

//点击Home键。
runRoot("input keyevent 3");

//点击任务键。
runRoot("input keyevent 82");

//点击锁屏键。
runRoot("input keyevent 26");

等等一堆命令请自行百度：android 常用adb 命令

4.3.7版本开始支持的功能：
var result=adbWithResult("你的root命令");
用adbWithResult方法，可以得到adb命令执行后的结果返回


```

　

　

### 4.2.7版本开始支持的功能：

### 执行HID命令，支持闪灵、蜂群、Rain

可以执行HID键码命令。

```js

keyPressCode("键码数字");

例如：
闪灵的键码参考
地址 https://vimsky.com/examples/usage/arduino-language-functions-usb-keyboard-keyboardmodifiers-ar.html
![img](ckjl/1735783840868_101.png)
![img](ckjl/1735783854599_102.png)
![img](ckjl/1735783864959_103.png)
![img](ckjl/1735783874711_104.png)
![img](ckjl/1735783893997_105.png)
![img](ckjl/1735783902327_106.png)
![img](ckjl/1735783910243_107.png)


```

　

### 4.6.7版本开始支持的功能：

```js

// 基础打开链接
openLink({ data: "https://www.baidu.com" });


// 单参数：只传按键名
pcClickKey("enter");
pcClickKey("escape");
pcClickKey("space");

// 双参数：按键名 + 持续时间(ms)
pcClickKey("ctrl+c", 100);
pcClickKey("tab", 200);
pcClickKey("backspace", 50);

// 双参数：x坐标, y坐标（字符串形式）
pcClickRight("500", "300");

// 三参数：x坐标, y坐标, 持续时间(ms)
pcClickRight("500", "300", 100);
pcClickRight("100", "200", 200);

// 返回键
back();

// 返回桌面
home();

// 最近任务
recents();

// 下拉通知栏
notifications();

// 下拉快捷设置
quickSettings();

// 电源菜单
powerDialog();

// 锁屏
lockScreen();

// 截图
takeScreenshot();

// 亮屏并解锁
wakeUpAndUnlock();

// 开启飞行模式 root手机才有效
enableAirplaneMode();

// 关闭飞行模式 root手机才有效
disableAirplaneMode();


// 播放文字，同时点击某个像素坐标，不填坐标就是不点击，duration代表按下多久，不填就是跟谁播放的文字长度自动按下时长；delay代表隔多久才播放文字，但是不影响按下，按下依旧是立马按下，只是播放文字慢2000毫秒再开始播放
speakText("你好", {x: '540', y: '960', duration: 3000, delay: 2000})
speakText("你好", {x: '50%', y: '70%', duration: 3000})
speakText("你好", {x: '540', y: '960', delay: 2000})
speakText("你好", {x: '50%', y: '80%'})

// 播放网络声音，同时点击某个坐标，不填坐标就是不点击，延迟2秒后播放，除了声音的网络地址，其他都可以不填
playNetAudio("http://example.com/a.mp3", {x: '540', y: '350', duration: 3000, delay: 2000})
playNetAudio("http://example.com/a.mp3", {x: '50%', y: '50%', delay: 2000})
playNetAudio("http://example.com/a.mp3")

// 播放铃声
playRingtone()

```

### 4.6.7版本开始支持的功能

```js
4.6.7版本开始支持的功能

// 最简调用：直接传文本字符串
let result = input("你好世界");

// 对象参数：只传 text
result = input({ text: "Hello World" });

// 使用剪切板粘贴方式输入（useClipboard: true）
result = input({ text: "批量内容", useClipboard: true });

// 使用五笔输入法打字（useWubi: true）
result = input({ text: "汉字内容", useWubi: true });

// 电脑端模式下不使用PC输入（noPcInput: true）
result = input({ text: "内容", noPcInput: true });

// inputType指定在输入框的原内容后面追加内容
result = input({ text: "重复内容", inputType: 1 });

// 组合参数：五笔 + 不用PC端 + 在输入框的原内容后面追加内容
result = input({ text: "测试文字", useWubi: true, noPcInput: true, inputType: 1 });

// 使用剪切板
result = input({ text: "快速输入", useClipboard: true });

// 返回值解析（input 返回 JSON 字符串）
let resultJson = input({ text: "测试" });
let resultObj = JSON.parse(resultJson);

```

### 4.6.7版本开始支持的功能

```js
4.6.7版本开始支持的功能

// 基础：只传图片路径/base64
let result = imgMatch({ text: "/sdcard/target.png" });

// 指定搜索区域（left/top/right/bottom 比例或像素）
result = imgMatch({
    text: "/sdcard/target.png",
    left: 0,
    top: 0,
    right: 1080,
    bottom: 600
});


// 指定相似度（sim，0~100）默认90
result = imgMatch({
    text: "/sdcard/icon.png",
    sim: 90
});

// 指定图片预处理类型，0默认不处理
result = imgMatch({
    text: "/sdcard/icon.png",
    imagePreType: 0
});

// 指定颜色过滤（perargb 数组）
result = imgMatch({
    text: "/sdcard/icon.png",
    imagePreType: 513,
    perargb: [255, 0, 0, 255]  // [a, r, g, b]
});

// 多图片匹配（text 传数组）
result = imgMatch({
    text: ["/sdcard/img1.png", "/sdcard/img2.png"],
    sim: 85
});

// 全参数组合
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
var location = JSON.parse(result);//json格式化
for (var i=0;i<location.length;i++)
{ 
    log('图片中心x坐标(像素)'+ location[i].x);
    log('图片中心y坐标(像素)'+ location[i].y);
    log('图片左边坐标(像素)'+ location[i].left);
    log('图片上边坐标(像素)'+ location[i].top);
    log('图片右边坐标(像素)'+ location[i].right);
    log('图片下边坐标(像素)'+ location[i].bottom);
}
imagePreType可选的配置数值为：
401,"X轴方向边缘检测处理",
402,"Y轴方向边缘检测处理",
403,"XY轴方向边缘检测处理",
404,"从左上往右下边缘检测处理",
405,"从右上往左下边缘检测处理",
406,"低阈值Canny边缘检测",
407,"高阈值Canny边缘检测",
408,"滤波后Canny边缘检测",
409,"Laplacian边缘检测",
410,"高斯滤波后Laplacian边缘检测",
411,"Scharr算子X边缘检测",
412,"Scharr算子Y边缘检测",
413,"Scharr算子XY边缘检测",
414,"boxFilter归一化",
415,"boxFilter未归一化",
416,"sqrBoxFilter归一化",
417,"脱色RGB2GRAY",
418,"脱色DeColor",
419,"脱色ColorBoosting",
420,"截图加70%透明黑色遮罩",
421,"目标图加70%透明黑色遮罩",
501,"提取黑色",
502,"提取灰色",
503,"提取白色",
504,"提取红色",
505,"提取橙色",
506,"提取黄色",
507,"提取绿色",
508,"提取青色",
509,"提取蓝色",
512,"提取自定义颜色",
510,"提取紫色",
513,"HSV色值调整",

```

### 4.6.7版本开始支持的功能

```js
// 跳转步骤，跳出去就等于js代码后面的也不会支持
gotoStep(3); // 跳转到第3步（步骤从1开始）

// 暂停流程（弹窗，用户点确定后继续执行后面的代码）
pauseFlow(); // 使用默认提示语
pauseFlow("请确认操作后再继续"); // 自定义提示语

// 暂停后继续执行后面的代码
log("开始执行");
pauseFlow("检查一下，没问题就点确定");
log("用户点了确定，继续执行"); // 用户点OK后才会执行到这里

// 终止当前流程
stopFlow();
// stopFlow() 之后的代码不会执行

// 终止全部流程
stopAllFlow();
// stopAllFlow() 之后的代码不会执行


// 普通词库(js代码中用到的词库在云备份和打包时都不会自动带上，所以你需要在某个步骤的变量控制中用一用这个词库才会自动带上)
var json = getWordLibrary("我的词库标题");
log("普通词库原始数据：" + json);

var words = JSON.parse(json);
log("普通词库共 " + words.length + " 条");
for (var i = 0; i < words.length; i++) {
    log("第" + (i + 1) + "条：" + words[i]);
}


// 关键词词库(js代码中用到的词库在云备份和打包时都不会自动带上，所以你需要在某个步骤的变量控制中用一用这个词库才会自动带上)
var json2 = getKeyWordLibrary("我的关键词词库标题");
log("关键词库原始数据：" + json2);

var items = JSON.parse(json2);
log("关键词词库共 " + items.length + " 组");
for (var i = 0; i < items.length; i++) {
    var item = items[i];
    log("第" + (i + 1) + "组 关键词：" + JSON.stringify(item.keys));
    log("第" + (i + 1) + "组 内容：" + JSON.stringify(item.contents));
}


//屏幕截图（内存截图）截图后用完记得要释放recycle(imgId);  否则占用内存；如果截图后30秒内不用的话，也会自动释放
let imgId = captureScreen();
if (imgId == null) {
    log("截屏失败");
    return;
}
log("截屏成功，图片ID=" + imgId);

//通过通过屏幕截图的imgId进行提取图片中的颜色
try {
    let colorJson = pixel(imgId, 100, 200);
    let col = JSON.parse(colorJson);
  
    log('十六进制颜色值：' + col.hexColor);      // #FF0000
    log('颜色值：' + col.color);                 // -8217131
    log('无符号颜色值：' + col.unsignedColor);   // 4286750105
    log('RGB: R=' + col.r + ', G=' + col.g + ', B=' + col.b);  // RGB: R=255, G=0, B=0
    log('Alpha: ' + col.a);                      // 255
} finally {
//释放图片占用的内存
    recycle(imgId);
}


//获取图片宽高
let width = getImgWidth(imgId);
log('宽度：' + width + 'px');  // 宽度：1080px
let height = getImgHeight(imgId);
log('高度：' + height + 'px');  // 高度：1920px
```

　

### 4.6.9版本开始支持的功能

```js
// YOLO目标检测
// ========== 方式一：自动截屏检测 ==========
var result = yoloRunModel({
  model: '你的模型名字',   // 模型管理页面中的模型名字
  left: '10%', top: '20%', right: '90%', bottom: '80%',  // 限制识别区域，支持像素/dp/百分比
  score: 50               // 匹配度百分比，不传默认90
});

log('yolo检测结果：' + result);

if (result == null) {
  log('检测失败或未找到目标');
} else {
  var items = JSON.parse(result);
  log('检测到目标数量：' + items.length);
  for (var i = 0; i < items.length; i++) {
    log('--- 目标' + (i + 1) + ' ---');
    log('标签：' + items[i].label);
    log('相似度：' + items[i].score + '%');
    log('中心x坐标：' + items[i].centerX);
    log('中心y坐标：' + items[i].centerY);
    log('左边：' + items[i].left);
    log('上边：' + items[i].top);
    log('右边：' + items[i].right);
    log('下边：' + items[i].bottom);
  }
}


// ========== 方式二：复用已有截图（截一张图做多次检测时更高效）==========
var imgId = captureScreen();  // 先截图
log('截图ID：' + imgId);

if (imgId == null) {
  log('截图失败');
} else {
  var result2 = yoloRunModel({
    model: '你的模型名字',
    score: 50,
    imgId: imgId   // 传入截图ID，跳过自动截屏
  });

  // 用完立即释放，防止内存泄露
  recycle(imgId);

  log('yolo检测结果：' + result2);

  if (result2 == null) {
    log('检测失败或未找到目标');
  } else {
    var items2 = JSON.parse(result2);
    log('检测到目标数量：' + items2.length);
    for (var i = 0; i < items2.length; i++) {
      log('--- 目标' + (i + 1) + ' ---');
      log('标签：' + items2[i].label);
      log('相似度：' + items2[i].score + '%');
      log('中心x：' + items2[i].centerX + '  中心y：' + items2[i].centerY);
      log('区域：left=' + items2[i].left + ' top=' + items2[i].top
          + ' right=' + items2[i].right + ' bottom=' + items2[i].bottom);
    }
  }
}



// ========== readExcel / writeExcel 读写Excel ==========

// ========== 读取本地Excel ==========
var result = readExcel(JSON.stringify({
  filePath: '/sdcard/客户列表.xlsx',  // Excel文件完整路径
  sheetName: 'Sheet1',               // Sheet名称，不填则读第一个Sheet
  row: 2,                            // 行号，从1开始
  cols: [1, 2, 3]                    // 要读取的列号数组，从1开始
}));

var r = JSON.parse(result);
log('读取结果：' + result);

if (!r.success) {
  log('读取失败：' + r.msg);
} else {
  log('读取行号：' + r.row);
  log('第1列值：' + r.values[0]);
  log('第2列值：' + r.values[1]);
  log('第3列值：' + r.values[2]);
}


// ========== 写入本地Excel（覆盖指定行）==========
var result2 = writeExcel(JSON.stringify({
  filePath: '/sdcard/结果.xlsx',   // Excel文件完整路径
  sheetName: 'Sheet1',            // Sheet名称，不填则写第一个Sheet
  row: 3,                         // 行号，从1开始
  cells: [
    {col: 1, value: '张三'},      // col列号从1开始，value写入内容
    {col: 2, value: '已完成'},
    {col: 3, value: '2024-01-01'}
  ]
}));

var w = JSON.parse(result2);
log('写入结果：' + result2);

if (!w.success) {
  log('写入失败：' + w.msg);
} else {
  log('写入成功，行号：' + w.row);
}


// ========== 写入本地Excel（row=-1 追加到末尾）==========
var result3 = writeExcel(JSON.stringify({
  filePath: '/sdcard/结果.xlsx',
  row: -1,                         // -1表示追加到最后一行之后
  cells: [
    {col: 1, value: '李四'},
    {col: 2, value: '处理中'}
  ]
}));

var w2 = JSON.parse(result3);
log('追加结果：' + result3);

if (!w2.success) {
  log('追加失败：' + w2.msg);
} else {
  log('追加成功，行号：' + w2.row);
}
```

```

```

　

### 更多功能正在开发中