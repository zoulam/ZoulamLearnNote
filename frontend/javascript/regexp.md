# 正则表达式

## 介绍

> ​	正则表达式是为了在匹配字符串的过程中减少if逻辑的书写达到减少代码量的效果，如账号、邮箱格式和密码强度匹配，以减少ajax请求，深拷贝等。
>
> 注：正则的书写需要配上完整的注释，因为阅读正则代码的效果，需要思考时间
>
> `/**/`块注释危险的原因：跟正则内容组合会让**块注释**失效
>
> ```text
> /*
>     let regexp = /a*/.match(s);
> */
> ```

### 	需要转义的字符表

```JavaScript
* . ? + $ ^ [ ] ( ) { } | \ / 
```

[正则解析网站](https://regexper.com/#%2F^1[3-9]\d{9}%2Fg)

## 1、创建方式和匹配模式（状态）

| 模式 | 效果 |
| :--- | :--- |
| `g` | 全局**global** |
| `i` | 不区分大小写**ignore** |
| `m` | 多行**multi-line** |

```javascript
// 字面量
var re = /wh/g;
// new对象
var re2 = new RegExp("wh");
```

## 2、function（`exec()`、`test()`）

| function | 语法，效果 |
| :--- | :--- |
| `exec` | `regexp.exec(string)` 返回一个数组 |
| `test` | `regexp.test(string)`返回`boolean` 即测试是否存在 |

```javascript
// 字面量
var re = /wh/;  //g是全局搜索这样就不会只匹配第一个
// new对象
var re2 = new RegExp("wh");
var str = "where when what";
//执行方式
//exec 返回详细信息
console.log(re.exec(str));
//[ 'wh', index: 0, input: 'where when what', groups: undefined ]
console.log(re2.exec(str));
//[ 'wh', index: 0, input: 'where when what', groups: undefined ]
//return ture/false
console.log(re.test(str));//true
console.log(re2.test(str));//true
```

```javascript
var str = "where when what";
var re = /wh/g;  //g是全局搜索这样就不会只匹配第一个
var re2 = new RegExp("wh");

console.log(re.exec(str));
// [ 'wh', index: 0, input: 'where when what', groups: undefined ]
console.log(re.exec(str));
// [ 'wh', index: 6, input: 'where when what', groups: undefined ]
console.log(re.exec(str));
// [ 'wh', index: 11, input: 'where when what', groups: undefined ]
console.log(re.exec(str));
//null


//又从头开始
console.log(re.exec(str));
//[ 'wh', index: 0, input: 'where when what', groups: undefined ]
```

### exec属性

```javascript
let text = "mom and dad and baby";
let reg = /mom( and dad( and baby)?)?/gi;
let result = reg.exec(text);

console.log(result.index);
// 0
console.log(result.input);
// mom and dad and baby
console.log(result[0]);
// mom and dad and baby
console.log(result[1]);
//  and dad and baby
console.log(result[2]);
//  and baby
console.log(result.groups);
// undefined
```

## 3、字符串匹配

```javascript
// 03字符串匹配.js

var str =`This str contains 123
CAPITALIZED letters  and _-&^% symbol`;

//需要完整匹配，多或者少都不行
console.log(/T/.test(str));//true
console.log(/This/.test(str));//true
console.log(/Thiss/.test(str));//false
console.log(/12/.test(str));//true
console.log(/1234/.test(str));//false
console.log(/_-&/.test(str));//true
```

## 4、特殊字符串匹配\(`match()`\)

| 符号 | 效果 |
| :--- | :--- |
| `.` | 匹配除了`\r \n \t`的任意字符 |
| `\d` | 匹配数字，改成`\D`就是匹配除了数字之外的值，下面的两个也是如此 |
| `\w` | 数字、字母（大小写全部包括）**下划线**（其他符号不包含） |
| `\s` | 匹配`\r \n \t`这类空格字符**space**的缩写 |

```javascript
// 04特殊字符串匹配.js

//以找某种规则匹配

var str = `This Thas str contains 123
CAPITALIZED letters  and _-&^% symbol`;

//返回值是被匹配到的内容
//.代表（除了\r \n \t）任意一个字符
console.log(str.match(/Th.s/g));//[ 'This', 'Thas' ]
console.log(str.match(/1.3/g));//[ '123' ]

//字母变成大写就是相反的意思
//\d代表任意数字
console.log(str.match(/\d/g));//[ '1', '2', '3' ]

//\w  数字、字母（大小写全部包括）下划线

console.log(str.match(/\w/g));
// [
//     'T', 'h', 'i', 's', 'T', 'h', 'a', 's',
//     's', 't', 'r', 'c', 'o', 'n', 't', 'a',
//     'i', 'n', 's', '1', '2', '3', 'C', 'A',
//     'P', 'I', 'T', 'A', 'L', 'I', 'Z', 'E',
//     'D', 'l', 'e', 't', 't', 'e', 'r', 's',
//     'a', 'n', 'd', '_', 's', 'y', 'm', 'b',
//     'o', 'l'
//   ]

// \s 代表（\r \n \t）
console.log(str.match(/\s/g));
// [
//     ' ', ' ',  ' ',
//     ' ', '\n', ' ',
//     ' ', ' ',  ' ',
//     ' '
//   ]


//中文字符串匹配是使用unicode码比较的
console.log("你好".match(/\u4f60/g));
//[ '你' ]
```

## 5、获取匹配次数的\(`match()`\)

| 需要转义的字符 | 不转义时的效果 |
| :--- | :--- |
| `*` | 通配符（无限次数） |
| `+` | 最少出现一次，`t+,`匹配`t`最少出现一次的片段，如：`t`、`tt`、`ttt`…… |
| `{}` | 划定某个字符出现次数的匹配`t{2}`匹配t最少出现两个 |
|  | `\d{1,3}`匹配最少出现一个，最多出现三个的数字片段 |
|  | `\d{1,}`匹配最少出现1个，最多出现无穷个的数字，效果：匹配所有的数字并打印 |
| `?` | `t?`匹配t出现0次或1次（**非贪婪**） |
| `+` | `t+`匹配`t`最少出现一次的片段，如：`t`、`tt`、`ttt`…… |

```javascript
var str = `This Thas str contains 123
CAPITALIZED letters  and _-&^% symbol`;

var str2 = `abcdef This Thas str contains 123CAPITALIZED letters  and _-&^% symbol`;
//.无法匹配换行符
console.log(str.match(/This.*/g));
//[ 'This Thas str contains 123' ]

console.log(str2.match(/This.*/g));
// [ 'This Thas str contains 123CAPITALIZED letters  and _-&^% symbol' ]

console.log(str.match(/t+/g));
//[ 't', 't', 'tt' ]
console.log(str.match(/T+/g));
//[ 'T', 'T', 'T' ]

//0次或者一次
console.log(str.match(/x?/g));
// [
//     '', '', '', '', '', '', '', '', '', '', '', '',
//     '', '', '', '', '', '', '', '', '', '', '', '',
//     '', '', '', '', '', '', '', '', '', '', '', '',
//     '', '', '', '', '', '', '', '', '', '', '', '',
//     '', '', '', '', '', '', '', '', '', '', '', '',
//     '', '', '', '', ''
//   ]
console.log(str.match(/t?/g));
// [
//     '', '', '', '', '', '',  '',  '', '', '', '', 't',
//     '', '', '', '', '', 't', '',  '', '', '', '', '',
//     '', '', '', '', '', '',  '',  '', '', '', '', '',
//     '', '', '', '', '', 't', 't', '', '', '', '', '',
//     '', '', '', '', '', '',  '',  '', '', '', '', '',
//     '', '', '', '', ''
//   ]

//连续两个t
console.log(str.match(/t{2}/g));
//[ 'tt' ]

//连续的出现1到3个的数字
console.log(str.match(/\d{1,3}/g));
// [ '123' ]

//连续的出现1到2个的数字
console.log(str.match(/\d{1,2}/g));
//[ '12', '3' ]

//至少出现一个数字
console.log(str.match(/\d{1,}/g));
//[ '123' ]
```

## 6、区间逻辑和界定符（`match()`）

| 符号 | 效果 |
| :--- | :--- |
| `[]` | 指定区间\(编码的顺序\)如:`[0-9]`匹配0到9的数字 |
| `^` | 在区间符号中是取反的意思，如：`[^a-z]`匹配除了a到z之间的字符 |
|  | 不在分组符号内：`^apple`,匹配必须以apple**开头**的字符串 |
| `$` | 以什么**结尾**，`close$`匹配以close结尾的字符串 |
| `\` | 转义字符，将正则中有实际功能的字符转义成普通字符 |
| **\|**（这是一条竖线，或逻辑） | **a\|b** 匹配a或者b |

```javascript
var str = `This str contains 123CAPITALIZED letters  and _-&^% symbol`;

console.log(str.match(/[abc]/g));
//[ 'c', 'a', 'a', 'b' ]

//范围性匹配

console.log(str.match(/[a-z]/g));//匹配小写字母a到z
// [
//     'h', 'i', 's', 's', 't', 'r',
//     'c', 'o', 'n', 't', 'a', 'i',
//     'n', 's', 'l', 'e', 't', 't',
//     'e', 'r', 's', 'a', 'n', 'd',
//     's', 'y', 'm', 'b', 'o', 'l'
//   ]
console.log(str.match(/[A-Z]/g));//匹配大写字母A-Z
// [
//     'T', 'C', 'A', 'P',
//     'I', 'T', 'A', 'L',
//     'I', 'Z', 'E', 'D'
//   ]
console.log(str.match(/[0-9]/g));//匹配数字0到0-9
// [ '1', '2', '3' ]
// ^取反
console.log(str.match(/[^0-9]/g));//匹配除了数字0-9之外的字符
// [
//     'T', 'h', 'i', 's', ' ', 's', 't', 'r',
//     ' ', 'c', 'o', 'n', 't', 'a', 'i', 'n',
//     's', ' ', 'C', 'A', 'P', 'I', 'T', 'A',
//     'L', 'I', 'Z', 'E', 'D', ' ', 'l', 'e',
//     't', 't', 'e', 'r', 's', ' ', ' ', 'a',
//     'n', 'd', ' ', '_', '-', '&', '^', '%',
//     ' ', 's', 'y', 'm', 'b', 'o', 'l'
//   ]
//转义
console.log(str.match(/[\-_&\^]/g));//[ '_', '-', '&', '^' ]


//或逻辑
console.log("或逻辑");
console.log(str.match(/This|contains/g));//匹配This或contains

//中括号外的^ 取头
var str = "a this that this and that";
var str2 ="this that this and that";
console.log(str.match(/^this/g));//必须以this 开头
// null
console.log(str2.match(/^this/g));//必须以this 开头
// [ 'this' ]
console.log(str.match(/this/g));
// [ 'this', 'this' ]
//$取尾
console.log(str.match(/that$/g));//必须以that结尾
// [ 'that' ]

var str2="athat that thatb that";

console.log(str2.match(/that/g));//[ 'that', 'that', 'that', 'that' ]

console.log(str2.match(/\bthat\b/g));//边界设定
// [ 'that', 'that' ]
```

## 7、分组

| 符号 | 效果 |
| :--- | :--- |
| `()` | 分组，实现整段匹配 |

```javascript
var str = `this that this and that`;
console.log(/(th).*(tha)/.exec(str));
// [
//     'this that this and tha',
//     'th',
//     'tha',
//     index: 0,
//     input: 'this that this and that',
//     groups: undefined
//   ]
var str = `aaaab abb cddaa`;
console.log(str.match(/(aa){2}/g));
//[ 'aaaa' ]
```

## 8、简单练习①（手机、邮箱、账号）

```javascript
//手机验证
//首位是1 第二位从3开始，总共是11位数字
var mobileRe = /^1[3-9]\d{9}/g;
console.log(mobileRe.test("15363356011"));
// 以数字1爱看i头，后面的九位数字在3-9的范围内

//邮箱验证
//@前边和后边只能有 大小写字母 数字 下划线 中划线 点号
//@后边 必须有 点号 后面跟一个域名（com或者cn）

var emailRe = /^([a-zA-Z0-9_\-\.]+)@([a-zA-Z0-9_\-\.]+)\.([a-zA-Z]{2,5})$/g
console.log(emailRe.test("createournovel@gmail.com"));


//简单验证账号
//6-15个字符 字母开头 只允许出现大小写字母、数字、下划线
var usernameRe = /^[a-zA-Z][a-zA-Z0-9_]{5,14}$/g;

console.log(usernameRe.test("aaa"));
console.log(usernameRe.test("$aaa"));
console.log(usernameRe.test("aaacdb"));
```

## 9、简单练习②（修正错误`replace()`、`split()`、获取HTML标签内容）

```javascript
// 09字符串替换.js
var str = "Tish is1 1an 3apple";
console.log(str.replace("Tish", "This"));
// This is1 1an 3apple
console.log(str.replace(/\d+/g, ""));
// Tish is an apple

var tags = "html,css,javascript";
console.log(tags.split(","));//[ 'html', 'css', 'javascript' ]

// 场景：非相同的符号分隔就得用上正则
var str = "This | is , an & apple";
console.log(str.split(/\W+/g));
//[ 'This', 'is', 'an', 'apple' ]

var html = `<span>hello</span><div>world</div>`;

console.log(html.replace(/<[^>]*>([^<>]*)<\/[^>]*>/g,"$1"));
//output： helloworld

// 逻辑分析
//<         匹配左括号
//[^>]*     匹配不包括右括号  的内容出现0次或多次
//>         匹配右括号
//([^<>*])  第一个分组，匹配除了<> 括号之外的内容
//g         持续操作，直至匹配完整个字符串
//$1 是引用分组 ([^<>]*)
```

