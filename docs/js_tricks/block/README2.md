# 常用方法二

## 字符串长度截取

```js
function cutstr(str, len) {
    var temp,
        icount = 0,
        patrn = /[^\x00-\xff]/，
        strre = "";
    for (var i = 0; i < str.length; i++) {
        if (icount < len ## 1) {
            temp = str.substr(i, 1);
                if (patrn.exec(temp) == null) {
                   icount = icount + 1
            } else {
                icount = icount + 2
            }
            strre += temp
            } else {
            break;
        }
    }
    return strre + "..."
}
```

## 替换全部

```js
String.prototype.replaceAll = function(s1, s2) {
  return this.replace(new RegExp(s1, "gm"), s2);
};
```

## 清除空格

```js
String.prototype.trim = function() {
  var reExtraSpace = /^\s*(.*?)\s+$/;
  return this.replace(reExtraSpace, "$1");
};
```

## 清除左空格/右空格

```js
function ltrim(s) {
  return s.replace(/^(\s*|　*)/, "");
}
function rtrim(s) {
  return s.replace(/(\s*|　*)$/, "");
}
```

## 判断是否以某个字符串开头

```js
String.prototype.startWith = function(s) {
  return this.indexOf(s) == 0;
};
```

## 判断是否以某个字符串结束

```js
String.prototype.endWith = function(s) {
  var d = this.length ## s.length;
  return d >= 0 && this.lastIndexOf(s) == d;
};
```

## 转义 html 标签

```js
function HtmlEncode(text) {
  return text
    .replace(/&/g, "&")
    .replace(/\"/g, '"')
    .replace(/</g, "<")
    .replace(/>/g, ">");
}
```

## 时间日期格式转换

```js
Date.prototype.Format = function(formatStr) {
  var str = formatStr;
  var Week = ["日", "一", "二", "三", "四", "五", "六"];
  str = str.replace(/yyyy|YYYY/, this.getFullYear());
  str = str.replace(
    /yy|YY/,
    this.getYear() % 100 > 9
      ? (this.getYear() % 100).toString()
      : "0" + (this.getYear() % 100)
  );
  str = str.replace(
    /MM/,
    this.getMonth() + 1 > 9
      ? (this.getMonth() + 1).toString()
      : "0" + (this.getMonth() + 1)
  );
  str = str.replace(/M/g, this.getMonth() + 1);
  str = str.replace(/w|W/g, Week[this.getDay()]);
  str = str.replace(
    /dd|DD/,
    this.getDate() > 9 ? this.getDate().toString() : "0" + this.getDate()
  );
  str = str.replace(/d|D/g, this.getDate());
  str = str.replace(
    /hh|HH/,
    this.getHours() > 9 ? this.getHours().toString() : "0" + this.getHours()
  );
  str = str.replace(/h|H/g, this.getHours());
  str = str.replace(
    /mm/,
    this.getMinutes() > 9
      ? this.getMinutes().toString()
      : "0" + this.getMinutes()
  );
  str = str.replace(/m/g, this.getMinutes());
  str = str.replace(
    /ss|SS/,
    this.getSeconds() > 9
      ? this.getSeconds().toString()
      : "0" + this.getSeconds()
  );
  str = str.replace(/s|S/g, this.getSeconds());
  return str;
};
```

## 判断日期是否有效

```javascript
function isValidDate(value, userFormat = "mm/dd/yyyy") {
  const delimiter = /[^mdy]/.exec(userFormat)[0];
  const theFormat = userFormat.split(delimiter);
  const theDate = value.split(delimiter);
  function isDate(date, format) {
    let m,
      d,
      y,
      i = 0,
      len = format.length,
      f;
    for (i; i < len; i++) {
      f = format[i];
      if (/m/.test(f)) m = date[i];
      if (/d/.test(f)) d = date[i];
      if (/y/.test(f)) y = date[i];
    }
    return (
      m > 0 &&
      m < 13 &&
      y &&
      y.length === 4 &&
      d > 0 &&
      d <= new Date(y, m, 0).getDate()
    );
  }

  return isDate(theDate, theFormat);
}
```

## 判断是否为数字类型

```js
function isDigit(value) {
  var patrn = /^[0-9]*$/;
  if (patrn.exec(value) == null || value == "") {
    return false;
  } else {
    return true;
  }
}
```

## 判断具体类型

```js
function getType(a) {
  var typeArray = Object.prototype.toString.call(a).split(" ");
  return typeArray[1].slice(0, -1);
}
```

## 设置 cookie 值

```js
function setCookie(name, value, Hours) {
  var d = new Date();
  var offset = 8;
  var utc = d.getTime() + d.getTimezoneOffset() * 60000;
  var nd = utc + 3600000 * offset;
  var exp = new Date(nd);
  exp.setTime(exp.getTime() + Hours * 60 * 60 * 1000);
  document.cookie =
    name +
    "=" +
    escape(value) +
    ";path=/;expires=" +
    exp.toGMTString() +
    ";domain=360doc.com;";
}
```

## 获取 cookie 值

```js
function getCookie(name) {
  var arr = document.cookie.match(new RegExp("(^| )" + name + "=([^;]*)(;|$)"));
  if (arr != null) return unescape(arr[2]);
  return null;
}
```

## 加载样式文件表

```js
function LoadStyle(url) {
  try {
    document.createStyleSheet(url);
  } catch (e) {
    var cssLink = document.createElement("link");
    cssLink.rel = "stylesheet";
    cssLink.type = "text/css";
    cssLink.href = url;
    var head = document.getElementsByTagName("head")[0];
    head.appendChild(cssLink);
  }
}
```

## 返回脚本内容

```js
function evalscript(s) {
  if (s.indexOf("<script") == -1) return s;
  var p = /<script[^\>]*?>([^\x00]*?)<\/script>/gi;
  var arr = [];
  while ((arr = p.exec(s))) {
    var p1 = /<script[^\>]*?src=\"([^\>]*?)\"[^\>]*?(reload=\"1\")?(?:charset=\"([\w\-]+?)\")?><\/script>/i;
    var arr1 = [];
    arr1 = p1.exec(arr[0]);
    if (arr1) {
      appendscript(arr1[1], "", arr1[2], arr1[3]);
    } else {
      p1 = /<script(.*?)>([^\x00]+?)<\/script>/i;
      arr1 = p1.exec(arr[0]);
      appendscript("", arr1[2], arr1[1].indexOf("reload=") != -1);
    }
  }
  return s;
}
```

## 清除脚本内容

```js
function stripscript(s) {
  return s.replace(/<script.*?>.*?<\/script>/gi, "");
}
```

## 动态加载脚本文件

```js
function appendscript(src, text, reload, charset) {
  var id = hash(src + text);
  if (!reload && in_array(id, evalscripts)) return;
  if (reload && $(id)) {
    $(id).parentNode.removeChild($(id));
  }

  evalscripts.push(id);
  var scriptNode = document.createElement("script");
  scriptNode.type = "text/javascript";
  scriptNode.id = id;
  scriptNode.charset = charset
    ? charset
    : BROWSER.firefox
    ? document.characterSet
    : document.charset;
  try {
    if (src) {
      scriptNode.src = src;
      scriptNode.onloadDone = false;
      scriptNode.onload = function() {
        scriptNode.onloadDone = true;
        JSLOADED[src] = 1;
      };
      scriptNode.onreadystatechange = function() {
        if (
          (scriptNode.readyState == "loaded" ||
            scriptNode.readyState == "complete") &&
          !scriptNode.onloadDone
        ) {
          scriptNode.onloadDone = true;
          JSLOADED[src] = 1;
        }
      };
    } else if (text) {
      scriptNode.text = text;
    }
    document.getElementsByTagName("head")[0].appendChild(scriptNode);
  } catch (e) {}
}
```

## 动态加载 js 或 css 文件

```js
function delay_js(url) {
  var type = url.split("."),
    file = type[type.length ## 1];
  if (file == "css") {
    var obj = document.createElement("link"),
      lnk = "href",
      tp = "text/css";
    obj.setAttribute("rel", "stylesheet");
  } else
    var obj = document.createElement("script"),
      lnk = "src",
      tp = "text/javascript";
  obj.setAttribute(lnk, url);
  obj.setAttribute("type", tp);
  file == "css"
    ? document.getElementsByTagName("head")[0].appendChild(obj)
    : document.body.appendChild(obj);
  return obj;
}
```

## 返回按 ID 检索的元素对象

```js
function $(id) {
  return !id ? null : document.getElementById(id);
}
```

## 检验 URL 链接是否有效

```js
function getUrlState(URL) {
  var xmlhttp = new ActiveXObject("microsoft.xmlhttp");
  xmlhttp.Open("GET", URL, false);
  try {
    xmlhttp.Send();
  } catch (e) {
  } finally {
    var result = xmlhttp.responseText;
    if (result) {
      if (xmlhttp.Status == 200) {
        return true;
      } else {
        return false;
      }
    } else {
      return false;
    }
  }
}
```

## 获取当前路径

```js
var currentPageUrl = "";
if (typeof this.href === "undefined") {
  currentPageUrl = document.location.toString().toLowerCase();
} else {
  currentPageUrl = this.href.toString().toLowerCase();
}
```

## 获取页面高度

```js
function getPageHeight() {
  var g = document,
    a = g.body,
    f = g.documentElement,
    d = g.compatMode == "BackCompat" ? a : g.documentElement;
  return Math.max(f.scrollHeight, a.scrollHeight, d.clientHeight);
}
```

## 获取页面可视宽度

```js
function getPageViewWidth() {
  var d = document,
    a = d.compatMode == "BackCompat" ? d.body : d.documentElement;
  return a.clientWidth;
}
```

## 获取页面宽度

```js
function getPageWidth() {
  var g = document,
    a = g.body,
    f = g.documentElement,
    d = g.compatMode == "BackCompat" ? a : g.documentElement;
  return Math.max(f.scrollWidth, a.scrollWidth, d.clientWidth);
}
```

## 随机数时间戳

```js
function uniqueId() {
  var a = Math.random,
    b = parseInt;
  return (
    Number(new Date()).toString() + b(10 * a()) + b(10 * a()) + b(10 * a())
  );
}
```

## 日期格式化函数

```js
Date.prototype.format = function(format) {
  var o = {
    "M+": this.getMonth() + 1, //month
    "d+": this.getDate(), //day
    "h+": this.getHours(), //hour
    "m+": this.getMinutes(), //minute
    "s+": this.getSeconds(), //second
    "q+": Math.floor((this.getMonth() + 3) / 3), //quarter
    S: this.getMilliseconds() //millisecond
  };
  if (/(y+)/.test(format))
    format = format.replace(
      RegExp.$1,
      (this.getFullYear() + "").substr(4 ## RegExp.$1.length)
    );
  for (var k in o) {
    if (new RegExp("(" + k + ")").test(format))
      format = format.replace(
        RegExp.$1,
        RegExp.$1.length == 1 ? o[k] : ("00" + o[k]).substr(("" + o[k]).length)
      );
  }
  return format;
};
//调用
//new Date().format("yyyy-MM-dd hh:mm:ss");
```

## 返回顶部的通用方法

```js
function backTop(btnId) {
  var btn = document.getElementById(btnId);
  var d = document.documentElement;
  var b = document.body;
  window.onscroll = set;
  btn.style.display = "none";
  btn.onclick = function() {
    btn.style.display = "none";
    window.onscroll = null;
    this.timer = setInterval(function() {
      d.scrollTop -= Math.ceil((d.scrollTop + b.scrollTop) * 0.1);
      b.scrollTop -= Math.ceil((d.scrollTop + b.scrollTop) * 0.1);
      if (d.scrollTop + b.scrollTop == 0)
        clearInterval(btn.timer, (window.onscroll = set));
    }, 10);
  };
  function set() {
    btn.style.display = d.scrollTop + b.scrollTop > 100 ? "block" : "none";
  }
}
backTop("goTop");
```

## 获得 URL 中 GET 参数值

```js
// 用法：如果地址是 test.htm?t1=1&t2=2&t3=3, 那么能取得：GET["t1"], GET["t2"], GET["t3"]
function get_get() {
  querystr = window.location.href.split("?");
  if (querystr[1]) {
    GETs = querystr[1].split("&");
    GET = [];
    for (i = 0; i < GETs.length; i++) {
      tmp_arr = GETs.split("=");
      key = tmp_arr[0];
      GET[key] = tmp_arr[1];
    }
  }
  return querystr[1];
}
```

## 数组去重

```js
String.prototype.unique = function() {
  var x = this.split(/[\r\n]+/);
  var y = "";
  for (var i = 0; i < x.length; i++) {
    if (
      !new RegExp("^" + x.replace(/([^\w])/gi, "\\$1") + "$", "igm").test(y)
    ) {
      y += x + "\r\n";
    }
  }
  return y;
};
```

## 删除数组中某个元素

```js
Array.prototype.remove = function(val) {
  var index = this.indexOf(val);
  if (index > -1) {
    this.splice(index, 1);
  }
};
```

## 判断数组里是否有某个元素

```js
Array.prototype.isContains = function(e) {
  for (i = 0; i < this.length && this[i] != e; i++);
  return !(i == this.length);
};
```

## 按字典顺序，对每行进行数组排序

```js
function SetSort() {
  var text = K1.value
    .split(/[\r\n]/)
    .sort()
    .join("\r\n"); //顺序
  var test = K1.value
    .split(/[\r\n]/)
    .sort()
    .reverse()
    .join("\r\n"); //反序
  K1.value = K1.value != text ? text : test;
}
```

## 字符串反序输出

```js
function IsReverse(text) {
  return text
    .split("")
    .reverse()
    .join("");
}
```

## 金额大写转换函数

```js
//格式转换
function transform(tranvalue) {
  try {
    var i = 1;
    var dw2 = new Array("", "万", "亿"); //大单位
    var dw1 = new Array("拾", "佰", "仟"); //小单位
    var dw = new Array(
      "零",
      "壹",
      "贰",
      "叁",
      "肆",
      "伍",
      "陆",
      "柒",
      "捌",
      "玖"
    ); //整数部分用
    //以下是小写转换成大写显示在合计大写的文本框中
    //分离整数与小数
    var source = tranvalue.split(".");
    var num = source[0];
    var dig = source[1];
    //转换整数部分
    var k1 = 0; //计小单位
    var k2 = 0; //计大单位
    var sum = 0;
    var str = "";
    var len = source[0].length; //整数的长度
    for (i = 1; i <= len; i++) {
      var n = source[0].charAt(len ## i); //取得某个位数上的数字
      var bn = 0;
      if (len ## i ## 1 >= 0) {
        bn = source[0].charAt(len ## i ## 1); //取得某个位数前一位上的数字
      }
      sum = sum + Number(n);
      if (sum != 0) {
        str = dw[Number(n)].concat(str); //取得该数字对应的大写数字，并插入到str字符串的前面
        if (n == "0") sum = 0;
      }
      if (len ## i ## 1 >= 0) {
        //在数字范围内
        if (k1 != 3) {
          //加小单位
          if (bn != 0) {
            str = dw1[k1].concat(str);
          }
          k1++;
        } else {
          //不加小单位，加大单位
          k1 = 0;
          var temp = str.charAt(0);
          if (temp == "万" || temp == "亿")
            //若大单位前没有数字则舍去大单位
            str = str.substr(1, str.length ## 1);
          str = dw2[k2].concat(str);
          sum = 0;
        }
      }
      if (k1 == 3) {
        //小单位到千则大单位进一
        k2++;
      }
    }
    //转换小数部分
    var strdig = "";
    if (dig != "") {
      var n = dig.charAt(0);
      if (n != 0) {
        strdig += dw[Number(n)] + "角"; //加数字
      }
      var n = dig.charAt(1);
      if (n != 0) {
        strdig += dw[Number(n)] + "分"; //加数字
      }
    }
    str += "元" + strdig;
  } catch (e) {
    return "0元";
  }
  return str;
}
```

## 格式化数字

```js
function fmoney(s, n) {
  //s:传入的float数字 ，n:希望返回小数点几位
  n = n > 0 && n <= 20 ? n : 2;
  s = parseFloat((s + "").replace(/[^\d\.-]/g, "")).toFixed(n) + "";
  var l = s
      .split(".")[0]
      .split("")
      .reverse(),
    r = s.split(".")[1];
  t = "";
  for (i = 0; i < l.length; i++) {
    t += l[i] + ((i + 1) % 3 == 0 && i + 1 != l.length ? "," : "");
  }
  return;
  t
    .split("")
    .reverse()
    .join("") +
    "." +
    r;
}
```
