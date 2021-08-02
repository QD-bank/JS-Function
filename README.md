# 《JS函数的执行时机》

JS函数的调用时机不同，得到的结果不同
函数的执行结果是什么取决于函数在什么时机调用!
函数体本身写在哪跟执行结果无关!!!因为函数要执行了才会有结果!!!

## 案例1

```javascript
let a =1
function fn(){
    console.log(a)
}

```
问：打印出多少

答：不知道，因为函数还没有被调用

## 案例2

```javascript

let a = 1
function fn(){
    console.log(a)
}
fn()

```
这道题在案例1原有的基础上多了一个fn()函数调用，所有打印1

## 案例3

```javascript

let a = 1
function fn(){
    console.log(a)
}
a = 2
fn()

```
打印出2，因为a的值被修改了

## 案例4

```javascript

let a = 1
function fn(){
    console.log(a)
}
fn()
a = 2

```
打印出1，因为a的值被修改在函数调用之后

## 案例5

```javascript

let a = 1
function fn(){
  setTimeout(()=>{
    console.log(a)
  },0)
}

fn()
a = 2

```
根据前面的案例，这道题因该打印2

如果是这么想的，表示你在错误的道路上越走越远，因为这里出现了一个setTimeout，setTimeout()方法设置一个定时器，该定时器在定时器到期后执行一个函数或指定的一段代码。

所以这道题的输出结果为2

我们再看一个案例：

```javascript

let i = 0
for(i = 0; i < 6; i++){
    setTimeout(()=>{
        console.log(1)
    },0)
}

```
在忽略steTimeout()情况下，打印的结果应该是：0，1，2，3，4，5

因为有了setTimeout(),这道题的打印结果是6个6

setTimeout()通俗的说就是等忙完现在做的事情，在去做别的事情，在这个案例for循环就是现在做的事情，等for循环结束，i的值为6，所以打印出6个6

## 如何打印0，1，2，3，4，5

肯定有大聪明想到了这个答案：

```javascript

let  i = 0
for(i = 0; i < 6; i++ ){
    
    console.log(i)
}

```
哎，我不写setTimeout()不就行了，就是玩儿

## 方法1

我们可以通过for——let配合打印出0，1，2，3，4，5：

```javascript

for(let i = 0; i < 6; i++){
    setTimeout(()=>{
        console.log(i)
    },0)
}

```
这个代码在初始条件用let声明的i，在JS中for和let一起用会加东西，每次循环会多创建一个i

## 方法2

```javascript

let i
for(i = 0; i<6; i++){
    let x = i
    setTimeout(()=>{
      console.log(x)
    }，0)
}

```
新声明一个变量x，将i的值赋值给x，之后这个变量值怎么变化就与i之后的变化无关了

## 方法3

```javascript

let i
for(i = 0; i <6; i++){
    setTimeout((value)=>{
        console.log(value)
    },0,i)
}

```
利用setTimeout()函数的第三个参数，将i的值传入到函数中，那么之后就是输出i当时传进来时候的值，并不会跟随她之后的值而改变

## 方法四

```javascript

let i
for(i = 0; i < 6; i++){
    !function(j){
        setTimeout(()=>{
            console.log(j)
        },0)
    }(i)
}

```
使用立即执行函数，每次循环，就会立马执行这个匿名函数。所以匿名函数执行完之后，里面的j就确定了，之后就是过一会儿输出这个值的问题，就与之前的i怎么变化没有关系了