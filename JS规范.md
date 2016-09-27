
# JS代码规范

### 1.类型

原始值: 存取直接作用于它自身。
-  ```string```
-  ```number```
-  ```boolean```
-  ```null```
-  ```undefined```

```javascript
var a = 1;
var b = a;

b = 2;

console.log(a, b) // => 1, 2
```

复杂类型: 存取时作用于它自身值的引用
-  ```object```
-  ```array```
-  ```function```

```javascript
var a = [1, 2];
var b = a;

b[0] = 10;

console.log(a[0], b[0]) // => 10, 10
```

### 2.对象

2.1 [建议] 使用直接量创建对象
```javascript
// bad
var item = new Object();

// good
var item = {};
```

2.2 [强制] 禁止使用[保留字][1]作为属性名，因为在IE8及以下版本存在问题，使用同义词替换它
```javascript
// bad
var item = {
  class: 'dark'
};

// good
var item = {
  type: 'dark'
};
```

2.3 [强制] 禁止在对象的最后一个属性值后添加```,```，IE6和IE7下会存在js报错（同12.2）
```javascript
// bad
var item = {
  type: 'dark',
  element: 'flame',
};

// good
var item = {
  type: 'dark',
  element: 'flame'
};
```

### 3.数组

3.1 [建议] 使用直接量创建数组
```javascript
// bad
var item = new Array();

// good
var item = [];
```

3.2 [建议] 向数组末尾增加元素时，使用 Array#push 代替直接赋值
```javascript
var magicBook = [];

// bad
magicBook[magicBook.length] = 'Expelliarmus';

// good
magicBook.push('Expelliarmus');
```
3.3 [建议] [浅拷贝][4](shallow copy)数组时，使用 Array#slice

[浅拷贝][4]：由于对象和数组在赋值的时候都是引用传递，赋值的时候只是传递一个指针(见1.类型)，浅拷贝的数组中的对象依旧是引用。


[3]: http://jerryzou.com/posts/dive-into-deep-clone-in-javascript/

[4]: http://blog.csdn.net/yisuowushinian/article/details/45544343

```javascript
var items = [{ id: 1 },{ id: 2}];
var len = items.length;
var itemsCopy = [];
var i;

// bad
for (i = 0; i < len; i++) {
  itemsCopy[i] = items[i];
}

// good
itemsCopy = items.slice();

//is shallow copy?
items[0] == itemsCopy[0]; // => true 为浅拷贝
```

### 4.字符串

4.1 [强制] 使用单引号包裹字符串
```javascript
// bad
var name = "Harry Potter";

// good
var name = 'Harry Potter';
```

4.2 [建议] 超长的字符串使用连接符写成多行
```javascript
// bad
var spell = 'this is a very very long spell, and it is very very hard, you can not do it by yourself.';

// good
var spell = 'this is a very very long spell,' +
  'and it is very very hard,' +
  'you can not do it by yourself.';

```
4.3 [建议] 使用 Array#join 拼接字符串，性能更优
```javascript
var messageList = [
  {
    content: 'this is a pen'
  },{
    content: 'this is an another pen'
  },{
    content: 'this is also a pen'
  }
];
var length = messageList.length;

// bad
function inbox(messageList) {
  var items = '<ul>';

  for (i = 0; i < length; i++) {
    items += '<li>' + messageList[i].content + '</li>';
  }

  return items + '</ul>';
}

// good
function inbox(messageList) {
  var items = [];

  for (i = 0; i < length; i++) {
    items[i] = '<li>' + messageList[i].content + '</li>';
  }

  return '<ul>' + items.join('') + '</ul>';
}
```

### 5.函数

5.1 [强制] 禁止在非函数代码块中（如 ```if``` ```while```）声明函数
```javascript
// bad
if (flag) {
  function castSpell(){
    // do something...
  }
}

// good
function castAvada() {
  return 'Avada Kedavra';
}

var castSpell;

if (flag) {
  castSpell = castAvada;
}
```
5.2 [建议] 参数多于3个时，使用对象进行传参，这样扩展起来更加容易。
```javascript
// bad
function castSpell(witcher, spell, target, distance) {
  // do something...
}
castSpell('Harry Potter', 'Expelliarmus', 'Lord Voldemort', 60);

// good
function castSpell(params) {
  // do something...
}
castSpell({
  witcher: 'Harry Potter',
  spell: 'Expelliarmus',
  target: 'Lord Voldemort',
  distance: 60
});
```
5.3 [强制] 禁止把参数名命名为```arguments```，会取代函数中的```arguments```

5.4 [建议] 避免意料之外的变量提升，在执行函数前必须声明函数。
```javascript
// bad
castSpell();

function castSpell(){
  // do something...
}

// good

var castSpell = function(){
  // do something...
}
castSpell();

```

### 6.属性

6.1 [建议] 使用变量访问属性时，用中括号```[]```
```javascript
var magicBook = {
  simple: 'Expelliarmus',
  hard: ''
};

function getSpell(spell) {
  return magicBook[spell];
}

var spell = getSpell('simple');
```

### 7.变量

7.1 [强制] 总是使用 ```var``` 声明变量

7.2 [强制] 如果需要要声明一个全局变量，必须使用```window.```
```javascript
// bad
spell = 'Expelliarmus';

// good
window.spell = 'Expelliarmus';
```
7.3 [强制] 使用 ```var``` 声明每一个变量。这样做的好处是增加新变量将变的更加容易，而且你永远不用再担心调换错 ```;``` 跟 ```,```

```javascript
// bad
var spells = getSpells(),
    isCasting = true,
    witcher = 'Harry Potter';

// bad
// （这里造成了一个全局变量 witcher）
var spells = getSpells(),
    isCasting = true;
    witcher = 'Harry Potter';

// good
var spells = getSpells();
var isCasting = true;
var witcher = 'Harry Potter';
```
7.4 [建议] 最后声明未被赋值的变量，因为后续变量可能需要引用之前的变量
```javascript
// bad
var witcher;
var spells = getSpells();
var isCasting = true;


// good
var spells = getSpells();
var isCasting = true;
var witcher;
```

### 8.条件表达式和等号

8.1 条件表达式的强制类型转换遵循以下规则：

- 对象 被计算为 ```true```
- ```undefined``` 被计算为 ```false```
- ```null``` 被计算为 ```false```
- 布尔值 被计算为 布尔的值
- 数字 如果是 ```+0```, ```-0```, 或是 ```NaN``` 被计算为 ```false``` , 否则为 ```true```
- 字符串 如果是空字符串 ```''``` 则被计算为 ```false```, 否则为 ```true```

```javascript
if([0]) {
  // true 因为数组是对象，对象被计算为true
}

if([]){
 // true 因为数组是对象，对象被计算为true
}
```

8.2 [建议] 使用快捷方式
```javascript
// bad
if (name !== '') {
  // do something...
}

// good
if (name) {
  // do something...
}

// bad
if (flag === true) {
  // do something...
}

// good
if (flag) {
  // do something...
}

// bad
if (collection.length > 0) {
  // do something...
}

// good
if (collection.length) {
  // do something...
}
```

### 9.块

9.1 [强制] 所有多行的块使用大括号
```javascript
// bad
if (test)
  return false;

// good
if (test) return false;

// good
if (test) {
  return false;
}

// bad
function() { return false; }

// good
function() {
  return false;
}
```

### 10.注释

10.1 [建议] 单行注释需在被注释对象上方独占一行，注释前含一个空格。为保证代码清晰，一般对重要语句进行注释说明，如复杂逻辑或特殊变量说明。
```javascript
// bad

var characterName = 'Harry Potter'; // 角色名字是Harry Potter

// good

// 角色名字是Harry Potter
var characterName = 'Harry Potter';
```
10.2 [建议] 多行注释使用 /**...*/
```javascript
// bad

// 施放一个法术，并获得其施放效果。
// @param  <string> spell
// @return <string> effect
function castSpell(spell) {
  // do something...
  return effect;
}

// good

/**
 * 施放一个法术，并获得其施放效果。
 * @param  <string> spell
 * @return <string> effect
 */
function castSpell(spell) {
  // do something...
  return effect;
}
```


### 11.空白

11.1 [强制] 4个空格缩进

11.2 [建议] 在花括号前加一个空格
```javascript
// good
function test() {
  console.log('test');
}

// good
dog.set('attr', {
  age: '1 year',
  breed: 'Bernese Mountain Dog'
});
```

11.3 [建议] 使用空格把运算符隔开
```js
// bad
var x=x+y;
// good
var x = x + y;
```

11.4 [建议] `return`语句前加一行空行

11.5 [建议] 出现大段赋值时，等号对齐
```js
function getDom(op){
  var input1     = op.input1,
      inputBox1  = op.inputBox1,
      selectBox1 = op.selectBox1;
}
```

### 12.逗号
12.1 [强制] 在变量或表达式后面添加逗号，不要出现逗号在前
```js
// bad
var fruits = [
  apple
  , orange
  , pear
]
```

12.2 [强制] 去掉不必要的逗号，这在IE67下会出现怪异行为。
```js
// bad
var hero = {
  name:'hongxin',
  age:18,
}
var heros = [
  'Batman',
  'Superman',
]
```

### 13.分号
13.1 [强制] 每条语句必须加分号。

13.2 [建议] js文件中的自执行函数开头加分号，避免合并时的怪异问题。

```javascript
// 括号开头
;(function(){

})();
```

### 14.类型转换

14.1 字符串
[建议] 在语句开始时执行类型转换。
```js
var numNine = 9;
// 9 -> '9'
var stringNine = '' + numNine;
```

14.2 数字

[强制]使用parseInt转换数字时总是带上转换基数

```js
// bad
parseInt('0x10'); // => 16

parseInt('09'); // => 0 (ie8下, ie9+及chrome为9)

// good
parseInt('09',10); // => 9
```

14.3 [建议] 尽量规避使用位操作，因为会导致意外行为，除非你知道你所做的位操作是安全的
```js
2147483647 >> 0 //=> 2147483647
2147483648 >> 0 //=> -2147483648
```

14.4 布尔
```js
var flag = 0;

// good
var hasOne = !!flag;
```

### 15.命名规则
15.1 [建议] 命名应具有语义化，切勿出现太抽象的名称，如myObj  aVar， 除了循环中可能用到的i，j等

```javascript
// bad
var myArray = [];
var specialString = '';

function goon(){
  return;
}

// good
var toys = [];
var communityName = '';

function modifyName(){
  return;
}
```

15.2 [建议] 以驼峰形式命名变量、函数和实例，变量和实例应尽量以名词为主，函数名尽量以动词+名词组合，返回布尔值的函数尽量以is、has打头

```javascript

// good
var communityList = [];

function collectShells(){
  return shells;
}

function isAnimal(){
  return bool;
}

```

15.3 [建议] 使用帕斯卡式命名构造函数或类，如Animal、People等



15.4 [建议] 在模块开发中，抛出的类名应与文件名保持一致（即一个文件一个模块）
```js
// good
var CheckBox = require('./CheckBox')
```



### 16.构造函数
16.1 [建议] 给对象原型分配方法时，不要覆写原型链
```js
function CheckBox(){
  this.value = '';
}
// bad
CheckBox.prototype = {
  getValue:...,
  setValue:...
}
// good
CheckBox.prototype.getValue = function getValue(){
  return this.value;
}
Checkbox.prototype.setValue = function setValue(val){
  this.value = val;
}
```

16.2 [强制] 禁止修改内置对象中定义的方法，如toString()、valueOf()等，除非你明确知道你要干什么
```js
// very very bad
Objext.prototype.toString = function(){...}
```


### 17.模块
17.1 [建议] 模块的名称应尽量与文件名保持一致

17.2 [建议] 以IIFE（立即执行函数）封装模块，并在IIFE前后添加分号


### 18.Zepto
18.1 [建议] 使用$作为存储Zepto对象的变量名前缀
```js
// good
var $sideBar = $('.sidebar');
```

18.2 [建议] 缓存jQuery查询

