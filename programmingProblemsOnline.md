# 在线编程题

## 1.查找数组元素位置
```javascript
function indexOf(arr, item) {
  if (Array.prototype.indexOf){
      return arr.indexOf(item);
  } else {
      for (var i = 0; i < arr.length; i++){
          if (arr[i] === item){
              return i;
          }
      }
  }     
  return -1;
}
```
## 2.数组求和
### 不考虑算法复杂度，用递归做：
```javascript
function sum(arr) {
    var len = arr.length;
    if(len == 0){
        return 0;
    } else if (len == 1){
        return arr[0];
    } else {
        return arr[0] + sum(arr.slice(1));
    }
}
```
### 常规循环：
```javascript
function sum(arr) {
    var s = 0;
    for (var i=arr.length-1; i>=0; i--) {
        s += arr[i];
    }
    return s;
}
```
### 函数式编程 map-reduce：
```javascript
function sum(arr) {
    return arr.reduce(function(prev, curr, idx, arr){
        return prev + curr;
    });
}
```
### forEach遍历:
```javascript
function sum(arr) {
    var s = 0;
    arr.forEach(function(val, idx, arr) {
        s += val;
    }, 0);
    return s;
};
```
### eval：
```javascript
function sum(arr) {
    return eval(arr.join("+"));
};
```
## 3.移除数组中的元素

>题目要求不改变原数组，所以可以声明一个数组a用于保存arr中不同于item的值，最后将a返回。

```javascript
function remove(arr, item) {
     //声明一个新数组保存结果
     var a = [];
     //循环遍历
     for(var i=0; i < arr.length; i++){
         //如果arr[i]不等于item，就加入数组a
         if(arr[i] != item){
             a.push(arr[i]);
         }
     }
     return a;
 }
```
## 4.添加元素

>在数组 arr 末尾添加元素 item。不要直接修改数组 arr，结果返回新的数组。

```javascript
/**
 * 普通的迭代拷贝
 * @param arr
 * @param item
 * @returns {Array}
 */
var append = function(arr, item) {
    var length = arr.length,
        newArr = [];
 
    for (var i = 0; i < length; i++) {
        newArr.push(arr[i]);
    }
 
    newArr.push(item);
 
    return newArr;
};
 
/**
 * 使用slice浅拷贝+push组合
 * @param arr
 * @param item
 * @returns {Blob|ArrayBuffer|Array.<T>|string}
 */
var append2 = function(arr, item) {
    var newArr = arr.slice(0);  // slice(start, end)浅拷贝数组
    newArr.push(item);
    return newArr;
};
 
/**
 * 使用concat将传入的数组或非数组值与原数组合并,组成一个新的数组并返回
 * @param arr
 * @param item
 * @returns {Array.<T>|string}
 */
var append3 = function(arr, item) {
    return arr.concat(item);
};
```
## 5.删除数组第一个元素

>删除数组 arr 第一个元素。不要直接修改数组 arr，结果返回新的数组

```javascript
//利用slice
function curtail(arr) {
    return arr.slice(1);
}
//利用filter
function curtail(arr) {
    return arr.filter(function(v,i) {
        return i!==0;
    });
}
//利用push.apply+shift
function curtail(arr) {
    var newArr=[];
    [].push.apply(newArr, arr);
    newArr.shift();
    return newArr;
}
//利用join+split+shift    注意！！！：数据类型会变成字符型
function curtail(arr) {
    var newArr = arr.join().split(',');
    newArr.shift();
    return newArr;
}
//利用concat+shift 
function curtail(arr) {
    var newArr = arr.concat();
    newArr.shift();
    return newArr;
}
//普通的迭代拷贝
function curtail(arr) {
    var newArr=[];
    for(var i=1;i<arr.length;i++){
        newArr.push(arr[i]);
    }
    return newArr;
}
```
## 6.添加元素

>在数组 arr 的 index 处添加元素 item。不要直接修改数组 arr，结果返回新的数组。

```javascript
//利用slice+concat
function insert(arr, item, index) {
    return arr.slice(0,index).concat(item,arr.slice(index));
}
//利用concat +splice
function insert(arr, item, index) {
    var newArr=arr.concat();
    newArr.splice(index,0,item);
    return newArr;
}
//利用slice+splice
function insert(arr, item, index) {
    var newArr=arr.slice(0);
    newArr.splice(index,0,item);
    return newArr;
}
//利用push.apply+splice
function insert(arr, item, index) {
    var newArr=[];
    [].push.apply(newArr, arr);
    newArr.splice(index,0,item);
    return newArr;
}
//普通的迭代拷贝
function insert(arr, item, index) {
    var newArr=[];
    for(var i=0;i<arr.length;i++){
        newArr.push(arr[i]);
    }
    newArr.splice(index,0,item);
    return newArr;
}
```
## 7.查找重复元素

>将传入的数组arr中的每一个元素value当作另外一个新数组b的key，然后遍历arr去访问b[value]，若b[value]不存在，则将b[value]设置为1，若b[value]存在，则将其加1。

>可以想象，若arr中数组没有重复的元素，则b数组中所有元素均为1；若arr数组中存在重复的元素，则在第二次访问该b[value]时，b[value]会加1，其值就为2了。最后遍历b数组，将其值大于1的元素的key存入另一个数组a中，就得到了arr中重复的元素。

```javascript
function duplicates(arr) {
     //声明两个数组，a数组用来存放结果，b数组用来存放arr中每个元素的个数
     var a = [],b = [];
     //遍历arr，如果以arr中元素为下标的的b元素已存在，则该b元素加1，否则设置为1
     for(var i = 0; i < arr.length; i++){
         if(!b[arr[i]]){
             b[arr[i]] = 1;
             continue;
         }
         b[arr[i]]++;
     }
     //遍历b数组，将其中元素值大于1的元素下标存入a数组中
     for(var i = 0; i < b.length; i++){
         if(b[i] > 1){
             a.push(i);
         }
     }
     return a;
 }
```
## 8.时间格式化输出

```javascript
function formatDate(t,str){
  var obj = {
    yyyy:t.getFullYear(),
    yy:(""+ t.getFullYear()).slice(-2),
    M:t.getMonth()+1,
    MM:("0"+ (t.getMonth()+1)).slice(-2),
    d:t.getDate(),
    dd:("0" + t.getDate()).slice(-2),
    H:t.getHours(),
    HH:("0" + t.getHours()).slice(-2),
    h:t.getHours() % 12,
    hh:("0"+t.getHours() % 12).slice(-2),
    m:t.getMinutes(),
    mm:("0" + t.getMinutes()).slice(-2),
    s:t.getSeconds(),
    ss:("0" + t.getSeconds()).slice(-2),
    w:['日', '一', '二', '三', '四', '五', '六'][t.getDay()]
  };
  return str.replace(/([a-z]+)/ig,function($1){return obj[$1]});
}
```
## 9.斐波那契数列

>空间复杂度：o(n)，时间复杂度：o(n)，最快 o(1)

```javascript
var _fib = (function (n) {
   var memory = [0, 1];
   return function (n) {
       for (var i = memory.length; i <= n; i++) {
           memory[i] = memory[i - 1] + memory[i - 2];
       }
       //console.log(memory.length + ' numbers saved.');
       return memory.slice(0,n+1);
   };
})();

var fibonacci = function (n) {
   return _fib(n)[n];
}
```

>运用大数加法：

```javascript
var _fib = (function (n) {
   var memory = ['0', '1'];

   function add(a, b) {
       var res = '',
           c = 0;
       a = a.split('');
       b = b.split('');
       while (a.length || b.length || c) {
           c += ~~a.pop() + ~~b.pop();
           res = c % 10 + res;
           c = c > 9;
       }
       return res.replace(/^0+/, '');
   }

   return function (n) {
       for (var i = memory.length; i <= n; i++) {
           memory[i] = add(memory[i - 1], memory[i - 2]);
       }
       //console.log(memory.length + ' numbers saved.');
       return memory.slice(0, n + 1);
   };
})();
```

>优化内存管理：

```javascript
var _Fib = (function (n) {
   var memory = ['0', '1'];

   var add = function (a, b) {
       var res = '',
           c = 0;
       a = a.split('');
       b = b.split('');
       while (a.length || b.length || c) {
           c += ~~a.pop() + ~~b.pop();
           res = c % 10 + res;
           c = c > 9;
       }
       return res.replace(/^0+/, '');
   }

   return {
       get: function (n) {
           for (var i = memory.length; i <= n; i++) {
               memory[i] = add(memory[i - 1], memory[i - 2]);
           }
           //console.log(memory.length + ' numbers saved.');
           return memory.slice(0, n + 1);
       },
       clear: function () {
           //console.log('Memories reset.')
           memory = ['0', '1'];
       }
   };
})();

var fibonacci = function (n) {
   if (n < 0) {
       _Fib.clear();
   } else {
       return _Fib.get(n)[n];
   }
}


var fibonacci = function (n) {
   return _fib(n)[n];
}
```
## 10.颜色字符串转换

>将 rgb 颜色字符串转换为十六进制的形式，如 rgb(255, 255, 255) 转为 #ffffff

>1. rgb 中每个 , 后面的空格数量不固定

>2. 十六进制表达式使用六位小写字母

>3. 如果输入不符合 rgb 格式，返回原始输入

```javascript
function rgb2hex(sRGB) {
    var regexp=/rgb\((\d+),\s*(\d+),\s*(\d+)\)/;
    var ret=sRGB.match(regexp);
    if(!ret){
        return sRGB;
    }else{
        var str='#';
        for(var i=1;i<=3;i++){
            var m=parseInt(ret[i]);
            if(m<=255&&m>=0){
                str+=(m<16?'0'+m.toString(16):m.toString(16));
            }else{
                return sRGB;
            }
        }
        return str;
    }
}
```
## 11.将字符串转换为驼峰格式

>css 中经常有类似 background-image 这种通过 - 连接的字符，通过 javascript 设置样式的时候需要将这种样式转换成 backgroundImage 驼峰格式，请完成此转换功能

>1. 以 - 为分隔符，将第二个起的非空单词首字母转为大写

>2. -webkit-border-image 转换后的结果为 webkitBorderImage

```javascript
function cssStyle2DomStyle(sName) {
    return sName.replace(/(?!^)\-(\w)(\w+)/g, function(a, b, c){
            return b.toUpperCase() + c.toLowerCase();
        }).replace(/^\-/, '');
}
```

```javascript
function cssStyle2DomStyle(sName) {
    var arr = sName.split('');  
    //判断第一个是不是 '-'，是的话就删除 
    if(arr.indexOf('-') == 0)
        arr.splice(0,1);
   //处理剩余的'-'
    for(var i=0; i<arr.length; i++){
        if(arr[i] == '-'){
            arr.splice(i,1);
            arr[i] = arr[i].toUpperCase();
        }
    }
    return arr.join('');
}
```

## 12.求二次方

>为数组 arr 中的每个元素求二次方。不要直接修改数组 arr，结果返回新的数组

```javascript
function square(arr) {
    return arr.map(function(item,index,array){
        return item*item;
    })
}
```

```javascript
function square(arr) {
   //声明一个新的数组存放结果
     var a = [];
     arr.forEach(function(e){
         //将arr中的每一个元素求平方后，加入到a数组中
         a.push(e*e);
     });
     return a;
 }
```
## 13.避免全局变量

>要求：给定的 js 代码中存在全局变量，请修复：

>思路：在Javascript语言中，声明变量使用的都是关键字var，如果不使用var而直接声明变量，则该变量为全局变量。

```javascript
function globals() {
    //只需要在声明myObject时加上var就行了
    var myObject = {
      name : 'Jory'
    };
 
    return myObject;
}
```
## 14.将字符串转换为驼峰格式

>要求：修改 js 代码中 parseInt 的调用方式，使之通过全部测试用例。

```javascript
parseInt(string, radix)
//当参数 radix 的值为 0，或没有设置该参数时，parseInt() 会根据 string 来判断数字的基数。
```

>思路：举例，如果 string 以 "0x" 开头，parseInt() 会把 string 的其余部分解析为十六进制的整数。如果 string 以 0 开头，那么 ECMAScript v3 允许 parseInt() 的一个实现把其后的字符解析为八进制或十六进制的数字。如果 string 以 1 ~ 9 的数字开头，parseInt() 将把它解析为十进制的整数。

```javascript
function parse2Int(num)
{
    return parseInt(num,10);
}
//解释来自于W3school
```

## 15.计时器

>要求：实现一个打点计时器，要求：

>1、从 start 到 end（包含 start 和 end），每隔 100 毫秒 console.log 一个数字，每次数字增幅为 1；

>2、返回的对象中需要包含一个 cancel 方法，用于停止定时操作；

>3、第一个数需要立即输出。

>思路：setInterval() 方法会按照指定周期不停地调用函数，直到 clearInterval() 被调用或窗口被关闭。由 setInterval() 返回的 ID 值可用作 clearInterval() 方法的参数。注意第一个数需要立即输出即可。

```javascript
function count(start, end) {
  //立即输出第一个值
  console.log(start++);
     var timer = setInterval(function(){
         if(start <= end){
             console.log(start++);
         }else{
             clearInterval(timer);
         }
     },100);
    //返回一个对象
     return {
         cancel : function(){
             clearInterval(timer);
         }
     };
 }
```

## 16.函数的上下文

>要求：将函数 fn 的执行上下文改为 obj 对象

>思路：在JavaScript中，函数是一种对象，其上下文是可以变化的，对应的，函数内的this也是可以变化的，函数可以作为一个对象的方法，也可以同时作为另一个对象的方法，可以通过Function对象中的call或者apply方法来修改函数的上下文，函数中的this指针将被替换为call或者apply的第一个参数。将函数 fn 的执行上下文改为 obj 对象，只需要将obj作为call或者apply的第一个参数传入即可。

```javascript
function speak(fn, obj) {
  return fn.apply(obj, obj);
 }
```

## 17.流程控制

>要求：实现 fizzBuzz 函数，参数 num 与返回值的关系如下：

>1、如果 num 能同时被 3 和 5 整除，返回字符串 fizzbuzz2、如果 num 能被 3 整除，返回字符串 fizz

>3、如果 num 能被 5 整除，返回字符串 buzz

>4、如果参数为空或者不是 Number 类型，返回 false

>5、其余情况，返回参数 num

>思路：能否整除即余数是否为0，则使用%运算符。使用if-elseif结构，只要某一条匹配，则下面的不会在进行判断。判断num是否为Number，可以用typeof运算符，返回的是字符串。

```javascript
function fizzBuzz(num) {
    if(num%3 == 0 && num%5 == 0)
        return "fizzbuzz";
    else if(num%3 == 0)
        return "fizz";
    else if(num%5 == 0)
        return "buzz";
    else if(num == null || typeof num != "number")
        return false;
    else return num;
}
```

## 18.返回函数

>要求：实现函数 functionFunction，调用之后满足如下条件：

>1、返回值为一个函数 f

>2、调用返回的函数 f，返回值为按照调用顺序的参数拼接，拼接字符为英文逗号加一个空格，即 ', '

>3、所有函数的参数数量为 1，且均为 String 类型

>思路：首先执行functionFunction('Hello')，传入参数str，然后返回函数f，f与('world')组合，执行f('world')，传入参数s，f返回str+", "+s，即Hello, world。注意中间的逗号后面有一个空格。

```javascript
//链接：https://www.nowcoder.com/questionTerminal/1f9fd23cdfd14675ab10207191e1d035
//来源：牛客网

function functionFunction(str) {
  var f = function(s){
         return str+", "+s;
     }
     return f;
 }
 ```
 
## 19.二次封装函数

>要求：

>已知函数 fn 执行需要 3 个参数。请实现函数 partial，调用之后满足如下条件：

>1、返回一个函数 result，该函数接受一个参数

>2、执行 result(str3) ，返回的结果与 fn(str1, str2, str3) 一致

```javascript
// 一般写法
function partial(fn, str1, str2) {
    function result(str3) {
        return fn(str1, str2, str3);
    }
 
    return result;
}
 
// call
function partial(fn, str1, str2) {
    function result(str3) {
        return fn.call(this, str1, str2, str3);
    }
 
     return result;
}
 
// apply（这里只是为了对照）
function partial(fn, str1, str2) {
    function result(str3) {
        return fn.apply(this, [str1, str2, str3]);
    }
 
    return result;
}
 
// 这个bind会生成一个新函数对象, 它的str1, str2参数都定死了, str3未传入, 一旦传入就会执行
function partial(fn, str1, str2) {
    return fn.bind(this, str1, str2); // 或 return fn.bind(null, str1, str2);
}
 
// bind同上, 多了一步, 把str3传入的过程写在另一个函数里面, 而另一个函数也有str1, str2参数
function partial(fn, str1, str2) {
    function result(str3) {
        return fn.bind(this, str1, str2)(str3);
    }
 
    return result;
}
 
// 匿名函数
function partial(fn, str1, str2) {
    return function(str3) {
        return fn(str1, str2, str3);
    }
}
// ES6
const partial = (fn, str1, str2) => str3 => fn(str1, str2, str3);
```

## 20.使用 arguments

>要求：

>函数 useArguments 可以接收 1 个及以上的参数。请实现函数 useArguments，返回所有调用参数相加后的结果。本题的测试参数全部为 Number 类型，不需考虑参数转换。

>思路：

>本题考查的是对于arguments的使用，arguments能获得函数对象传入的参数组，类似与一个数组，能够通过length获取参数个数，能通过下标获取该位置的参数，但是它不能使用forEach等方法。本题先通过arguments.length获得参数个数，然后循环求和，得出结果。

```javascript
function useArguments() {
  /*
   因为参数数量不定，可以先获取参数个数arguments.length
   然后循环求值
  */
  //声明一个变量保存最终结果
  var sum = 0;
  //循环求值
  for(var i = 0; i < arguments.length; i++){
      sum += arguments[i];
  }
  return sum;
 }
```

## 21.二次封装函数

>要求：

>实现函数 partialUsingArguments，调用之后满足如下条件：

>1、返回一个函数 result

>2、调用 result 之后，返回的结果与调用函数 fn 的结果一致

>3、fn 的调用参数为 partialUsingArguments 的第一个参数之后的全部参数以及 result 的调用参数

>思路：

>arguments不能用slice方法直接截取，需要先转换为数组，var args = Array.prototype.slice.call(arguments);合并参数可以使用concat方法，并且也需要将arguments先转换为数组才能使用concat进行合并。最用使用apply执行传入的函数即可。

```javascript
function partialUsingArguments(fn) {
     //先获取p函数第一个参数之后的全部参数
     var args = Array.prototype.slice.call(arguments,1);
     //声明result函数
     var result = function(){
         //使用concat合并两个或多个数组中的元素
         return fn.apply(null, args.concat([].slice.call(arguments)));
     }
     return result;
 }
 ```
 
## 22.柯里化

>要求：

>已知 fn 为一个预定义函数，实现函数 curryIt，调用之后满足如下条件：

>1、返回一个函数 a，a 的 length 属性值为 1（即显式声明 a 接收一个参数）

>2、调用 a 之后，返回一个函数 b, b 的 length 属性值为 1

>3、调用 b 之后，返回一个函数 c, c 的 length 属性值为 1

>4、调用 c 之后，返回的结果与调用 fn 的返回值一致

>5、fn 的参数依次为函数 a, b, c 的调用参数

>思路：

>不用 arguments.callee 的方法，用变量名代替了，大同小异：

```javascript
function curryIt(fn) {
    var length = fn.length,
        args = [];
    var result =  function (arg){
        args.push(arg);
        length --;
        if(length <= 0 ){
            return fn.apply(this, args);
        } else {
            return result;
        }
    }
     
    return result;
}
```

## 23.且运算

>要求：

>返回参数 a 和 b 的逻辑且运算结果。

>思路：

>且运算符"&&"的运算规则是：如果第一个运算子的布尔值为true，则返回第二个运算子的值（注意是值，不是布尔值）；如果第一个运算子的布尔值为false，则直接返回第一个运算子的值，且不再对第二个运算子求值。

```javascript
function and(a, b) {
    return !!(a && b)
    //如果a为true，b为非Boolean就会返回非Boolean值，所以加一步转换
}
```

## 24.二进制转换

>要求：

>给定二进制字符串，将其换算成对应的十进制数字

>思路：

>parseInt方法可以将其它进制转换为十进制，只需要给该方法传入需要转换的字符串和该字符串的进制表示两个参数即可。

```javascript
function base10(str) {
    /**
        其它进制转十进制
        parseInt(str,2)
        parseInt(str,8)
        parseInt(str,16)
    */
    return parseInt(str,2);
}
```
## 25.改变上下文

>要求：

>将函数 fn 的执行上下文改为 obj，返回 fn 执行后的值。

>思路：

>在JavaScript中，函数是一种对象，其上下文是可以变化的，对应的，函数内的this也是可以变化的，函数可以作为一个对象的方法，也可以同时作为另一个对象的方法，可以通过Function对象中的call或者apply方法来修改函数的上下文，函数中的this指针将被替换为call或者apply的第一个参数。将函数 fn 的执行上下文改为 obj 对象，只需要将obj作为call或者apply的第一个参数传入即可。

```javascript
function alterContext(fn, obj) {
  return fn.call(obj,obj);
 }
```

## 26.判断是否包含数字

>要求：

>给定字符串 str，检查其是否包含数字，包含返回 true，否则返回 false

>思路：

>判断字符串中是否含有数字，可以用正则表达式。/\d/可以匹配字符串中的数字字符，用test方法可以检测。

```javascript
function containsNumber(str) {
     var b = /\d/;
     return b.test(str);
 }
```

## 27.属性遍历

>要求：

>找出对象 obj 不在原型链上的属性(注意这题测试例子的冒号后面也有一个空格~)

>1、返回数组，格式为 key: value

>2、结果数组不要求顺序

>思路：

>可以使用for-in来遍历对象中的属性，hasOwnproperty方法能返回一个布尔值，指出一个对象是否具有指定名称的属性。此方法无法检查该对象的原型链中是否具有该属性，该属性必须为对象本身的属性。

```javascript
function iterate(obj) {
     var arr = [];
     //使用for-in遍历对象属性
     for(var key in obj){
         //判断key是否为对象本身的属性
         if(obj.hasOwnProperty(key)){
             //将属性和值按格式存入数组
             arr.push(key+": "+obj[key]);
         }
     }
     return arr;
 }
 ```
 
## 28.检查重复字符串

>要求：

>给定字符串 str，检查其是否包含连续重复的字母（a-zA-Z），包含返回 true，否则返回 false

>思路：

>在正则表达式中，利用()进行分组，使用斜杠加数字表示引用，\1就是引用第一个分组，\2就是引用第二个分组。将[a-zA-Z]做为一个分组，然后引用，就可以判断是否有连续重复的字母。

```javascript
function containsRepeatingLetter(str) {
     return /([a-zA-Z])\1/.test(str);
 }
```

## 29.获取指定字符串

>要求：

>给定字符串 str，检查其是否包含 连续3个数字 1、如果包含，返回最新出现的 3 个数字的字符串 2、如果不包含，返回 false

>思路：

>题目描述有问题，实际考察的是字符串中是否含有连续的三个任意数字，而不是三个连续的数字。依题，若存在连续的三个任意数字，则返回最早出现的三个数字，若不存在，则返回false。因此需要用到match方法，match()返回的是正则表达式匹配的字符串数组，连续的三个任意数字用正则表达式表示为/\d{3}/。

```javascript
function captureThreeNumbers(str) {
     //声明一个数组保存匹配的字符串结果
  var arr = str.match(/\d{3}/);
     //如果arr存在目标结果，则返回第一个元素，即最早出现的目标结果
     if(arr)
         return arr[0];
     else return false;
 }
```

## 30.判断是否符合指定格式

>要求：

>给定字符串 str，检查其是否符合如下格式 1、XXX-XXX-XXXX 2、其中 X 为 Number 类型

>思路：

>本题需要注意格式，开头^和结尾$必须加上来限定字符串，3个数可表示为\d{3}，4个数则为\d{4}，{n}表示前面内容出现的次数。正则表达式可写作/^\d{3}-\d{3}-\d{4}$/，有相同部分\d{3}-，因此也可写作/^(\d{3}-){2}\d{4}$/

```javascript
function matchesPattern(str) {
    return/^(\d{3}-){2}\d{4}$/.test(str);
}
```
