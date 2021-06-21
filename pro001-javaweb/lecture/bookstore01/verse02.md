[TOC]

# 第二节 正则表达式

## 1、从凤姐的择偶标准说起

![images](images/img005.png)

本人对伴侣要求如下：
- 第一、必须为北京大学或清华大学硕士毕业生。必须本科硕士连读，中途无跳级，不留级，不转校。在外参加工作后再回校读书者免。
- 第二、必须为经济学专业毕业。非经济学专业毕业则必须精通经济学。或对经济学有浓厚的兴趣。
- 第三、必须具备国际视野，但是无长期定居国外甚至移民的打算。
- 第四、身高176--183左右。长得越帅越好。
- 第五、无生育史。过往所有女友均无因自身而致的堕胎史。
- 第六、东部户籍，即江、浙、沪三地户籍或黑龙江、广东、天津、山东、北京、吉林、辽宁等。
- 东北三省和内蒙古等地户籍，西南地区即重庆、贵州、云南、西藏和湖南、湖北等地籍贯者不予考虑。　
- 第七、年龄25--28岁左右。即06届，07届，08届，09届毕业生。有一至两年的工作经验，06级毕业生需年龄在28岁左右，09级毕业生则需聪明过人。且具备丰富的社会实践经验。就职于国家机关，国有企事业单位者不愿考虑。但就职于中石油，中石化等世界顶尖型企业或银行者又比较喜欢。现自主创业者要商榷一番了。



## 2、标准在手，世界我有

### ①模式验证

使用标准衡量一位具体的男士，返回一个布尔值，从而知道这位男士是否满足自己的标准——相当于我们使用正则表达式验证一个字符串是否满足规则。比如验证一个字符串是否是一个身份证号。



### ②匹配读取

对全中国的男士应用这个标准，返回一个数组，遍历这个数组，可以得到所有符合标准的男士——相当于我们使用正则表达式获取一段文本中匹配的子字符串。比如将一篇文章中的电子邮件地址读取出来。



### ③匹配替换

对全中国的男士应用这个标准，把其中已婚的变成未婚，这样凤姐就有机会了——相当于我们使用正则表达式替换所有匹配的部分。比如将一段文字中的”HelloWorld”替换为”HelloJava”。

> 花絮：
>
> 记者：封老师您好！由于您的名字『封捷』和『凤姐』谐音，同学们总是以此来调侃您，说您是尚硅谷『凤姐』，对此您有什么想说的吗？
>
> 封老师：太过分了！我咋能和人家比！
>
> 记者：呃……太意外了，您的意思是？
>
> 封老师：虽然过气了，但人家好歹也是网红呀！



## 3、正则表达式的概念

使用一段<span style="color:blue;font-weight:bold;">字符串</span>定义的一个<span style="color:blue;font-weight:bold;">规则</span>，用以<span style="color:blue;font-weight:bold;">检测</span>某个字符串是否满足这个规则，或将目标字符串中满足规则的部分<span style="color:blue;font-weight:bold;">读取</span>出来，又或者将目标字符串中满足标准的部分<span style="color:blue;font-weight:bold;">替换</span>为其他字符串。所以正则表达式有三个主要用途：

- 模式验证
- 匹配读取
- 匹配替换

![images](images/img006.png)



## 4、正则表达式零起步

### ①创建正则表达式对象

#### [1]使用两个斜杠

```javascript
// 类似创建数组时可以使用[]、创建对象可以使用{}
var reg = /a/;
```



#### [2]使用new关键字创建RegExp类型的对象

```javascript
// 类似创建数组可以new Array()、创建对象可以使用new Object()
var reg = new RegExp("a");
```



### ②正则表达式的组成

正则表达式本身也是一个字符串，它由两种字符组成：

- 普通字符，例如大、小写英文字母；数字等。
- 元字符：被系统赋予特殊含义的字符。例如：^表示以某个字符串开始，$表示以某个字符串结束。



### ③正则表达式初体验

#### [1]模式验证

<span style="color:blue;font-weight:bold;">注意</span>：这里是使用<span style="color:blue;font-weight:bold;">正则表达式对象</span>来<span style="color:blue;font-weight:bold;">调用</span>方法。

```javascript
// 创建一个最简单的正则表达式对象
var reg = /o/;

// 创建一个字符串对象作为目标字符串
var str = 'Hello World!';

// 调用正则表达式对象的test()方法验证目标字符串是否满足我们指定的这个模式，返回结果true
console.log("/o/.test('Hello World!')="+reg.test(str));
```



#### [2]匹配读取

<span style="color:blue;font-weight:bold;">注意</span>：这里是使用<span style="color:blue;font-weight:bold;">字符串对象</span>来<span style="color:blue;font-weight:bold;">调用</span>方法。

```javascript
// 在目标字符串中查找匹配的字符，返回匹配结果组成的数组
var resultArr = str.match(reg);
// 数组长度为1
console.log("resultArr.length="+resultArr.length);

// 数组内容是o
console.log("resultArr[0]="+resultArr[0]);
```



#### [3]替换

<span style="color:blue;font-weight:bold;">注意</span>：这里是使用<span style="color:blue;font-weight:bold;">字符串对象</span>来<span style="color:blue;font-weight:bold;">调用</span>方法。

```javascript
var newStr = str.replace(reg,'@');
// 只有第一个o被替换了，说明我们这个正则表达式只能匹配第一个满足的字符串
console.log("str.replace(reg)="+newStr);//Hell@ World!

// 原字符串并没有变化，只是返回了一个新字符串
console.log("str="+str);//str=Hello World!
```



### ④匹配方式

#### [1]全文查找

如果不使用g对正则表达式对象进行修饰，则使用正则表达式进行查找时，仅返回第一个匹配；使用g后，返回所有匹配。

```javascript
// 目标字符串
var targetStr = 'Hello World!';

// 没有使用全局匹配的正则表达式
var reg = /[A-Z]/;
// 获取全部匹配
var resultArr = targetStr.match(reg);
// 数组长度为1
console.log("resultArr.length="+resultArr.length);

// 遍历数组，发现只能得到'H'
for(var i = 0; i < resultArr.length; i++){
	console.log("resultArr["+i+"]="+resultArr[i]);
}
```

对比代码：

```javascript
// 目标字符串
var targetStr = 'Hello World!';

// 使用了全局匹配的正则表达式
var reg = /[A-Z]/g;
// 获取全部匹配
var resultArr = targetStr.match(reg);
// 数组长度为2
console.log("resultArr.length="+resultArr.length);

// 遍历数组，发现可以获取到“H”和“W”
for(var i = 0; i < resultArr.length; i++){
	console.log("resultArr["+i+"]="+resultArr[i]);
}
```



#### [2]忽略大小写

```javascript
//目标字符串
var targetStr = 'Hello WORLD!';

//没有使用忽略大小写的正则表达式
var reg = /o/g;
//获取全部匹配
var resultArr = targetStr.match(reg);
//数组长度为1
console.log("resultArr.length="+resultArr.length);
//遍历数组，仅得到'o'
for(var i = 0; i < resultArr.length; i++){
	console.log("resultArr["+i+"]="+resultArr[i]);
}
```

对比代码：

```javascript
//目标字符串
var targetStr = 'Hello WORLD!';

//使用了忽略大小写的正则表达式
var reg = /o/gi;
//获取全部匹配
var resultArr = targetStr.match(reg);
//数组长度为2
console.log("resultArr.length="+resultArr.length);
//遍历数组，得到'o'和'O'
for(var i = 0; i < resultArr.length; i++){
	console.log("resultArr["+i+"]="+resultArr[i]);
}
```



#### [3]多行查找

不使用多行查找模式，目标字符串中不管有没有换行符都会被当作一行。

```javascript
//目标字符串1
var targetStr01 = 'Hello\nWorld!';
//目标字符串2
var targetStr02 = 'Hello';

//匹配以'Hello'结尾的正则表达式，没有使用多行匹配
var reg = /Hello$/;
console.log(reg.test(targetStr01));//false

console.log(reg.test(targetStr02));//true
```

对比代码：

```javascript
//目标字符串1
var targetStr01 = 'Hello\nWorld!';
//目标字符串2
var targetStr02 = 'Hello';

//匹配以'Hello'结尾的正则表达式，使用了多行匹配
var reg = /Hello$/m;
console.log(reg.test(targetStr01));//true

console.log(reg.test(targetStr02));//true
```



## 5、元字符

### ①概念

在正则表达式中被赋予特殊含义的字符，不能被直接当做普通字符使用。如果要匹配元字符本身，需要对元字符进行转义，转义的方式是在元字符前面加上“\”，例如：\^



### ②常用元字符

| 代码 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| .    | 匹配除换行字符以外的任意字符。                               |
| \w   | 匹配字母或数字或下划线等价于[a-zA-Z0-9_]                     |
| \W   | 匹配任何非单词字符。等价于\[^A-Za-z0-9_]                     |
| \s   | 匹配任意的空白符，包括空格、制表符、换页符等等。等价于[\f\n\r\t\v]。 |
| \S   | 匹配任何非空白字符。等价于\[^\f\n\r\t\v]。                   |
| \d   | 匹配数字。等价于[0-9]。                                      |
| \D   | 匹配一个非数字字符。等价于\[^0-9]                            |
| \b   | 匹配单词的开始或结束                                         |
| ^    | 匹配字符串的开始，但在[]中使用表示取反                       |
| $    | 匹配字符串的结束                                             |



#### [1]例1

```javascript
var str = 'one two three four';
// 匹配全部空格
var reg = /\s/g;
// 将空格替换为@
var newStr = str.replace(reg,'@'); // one@two@three@four
console.log("newStr="+newStr);
```



#### [2]例2

```javascript
var str = '今年是2014年';
// 匹配至少一个数字
var reg = /\d+/g;
str = str.replace(reg,'abcd');
console.log('str='+str); // 今年是abcd年
```



#### [3]例3

```javascript
var str01 = 'I love Java';
var str02 = 'Java love me';
// 匹配以Java开头
var reg = /^Java/g;
console.log('reg.test(str01)='+reg.test(str01)); // flase
console.log("<br />");
console.log('reg.test(str02)='+reg.test(str02)); // true
```



#### [4]例4

```javascript
var str01 = 'I love Java';
var str02 = 'Java love me';
// 匹配以Java结尾
var reg = /Java$/g;
console.log('reg.test(str01)='+reg.test(str01)); // true
console.log("<br />");
console.log('reg.test(str02)='+reg.test(str02)); // flase
```



## 6、字符集合

| 语法格式    | 示例                                                         | 说明                                               |
| ----------- | ------------------------------------------------------------ | -------------------------------------------------- |
| [字符列表]  | 正则表达式：[abc]<br />含义：目标字符串包含abc中的任何一个字符<br />目标字符串：plain<br />是否匹配：是<br />原因：plain中的“a”在列表“abc”中 | 目标字符串中任何一个字符出现在字符列表中就算匹配。 |
| [^字符列表] | [^abc]<br />含义：目标字符串包含abc以外的任何一个字符<br />目标字符串：plain<br />是否匹配：是<br />原因：plain中包含“p”、“l”、“i”、“n” | 匹配字符列表中未包含的任意字符。                   |
| [字符范围]  | 正则表达式：[a-z]<br />含义：所有小写英文字符组成的字符列表<br />正则表达式：[A-Z]<br />含义：所有大写英文字符组成的字符列表 | 匹配指定范围内的任意字符。                         |

```javascript
var str01 = 'Hello World';
var str02 = 'I am Tom';
//匹配abc中的任何一个
var reg = /[abc]/g;
console.log('reg.test(str01)='+reg.test(str01));//flase
console.log('reg.test(str02)='+reg.test(str02));//true
```



## 7、重复

| 代码  | 说明           |
| ----- | -------------- |
| *     | 重复零次或多次 |
| +     | 重复一次或多次 |
| ?     | 重复零次或一次 |
| {n}   | 重复n次        |
| {n,}  | 重复n次或多次  |
| {n,m} | 重复n到m次     |

```javascript
console.log("/[a]{3}/.test('aa')="+/[a]{3}/g.test('aa')); // flase
console.log("/[a]{3}/.test('aaa')="+/[a]{3}/g.test('aaa')); // true
console.log("/[a]{3}/.test('aaaa')="+/[a]{3}/g.test('aaaa')); // true
```



## 8、在正则表达式中表达『或者』

使用符号：|

```javascript
// 目标字符串
var str01 = 'Hello World!';
var str02 = 'I love Java';
// 匹配'World'或'Java'
var reg = /World|Java/g;
console.log("str01.match(reg)[0]="+str01.match(reg)[0]);//World
console.log("str02.match(reg)[0]="+str02.match(reg)[0]);//Java
```



## 9、常用正则表达式

| 需求     | 正则表达式                                              |
| -------- | ------------------------------------------------------- |
| 用户名   | /^\[a-zA-Z\_][a-zA-Z_\\-0-9]{5,9}$/                     |
| 密码     | /^[a-zA-Z0-9_\\-\@\\#\\&\\*]{6,12}$/                    |
| 前后空格 | /^\s+\|\s+$/g                                           |
| 电子邮箱 | /^[a-zA-Z0-9_\\.-]+@([a-zA-Z0-9-]+[\\.]{1})+[a-zA-Z]+$/ |



[上一节](verse01.html) [回目录](index.html) [下一节](verse03.html)