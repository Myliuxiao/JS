### 变量提升

所谓变量提升，就是当全局栈内存（作用域）形成，js代码自上而下执行之前，浏览器首先会把所有带“var/function”等关键词提前“声明”或者“定义”，这种预先处理机制称之为“变量提升”

- 声明（declare）：var a/function sum (默认值undefined)
- 定义（defined）：a = 2 (定义其实就是赋值操作）

[变量提升阶段]

- 带var的只声明为定义
- 带function的声明和赋值都完成了

变量提升只发生在当前作用域（例如：开始加载页面的时候只对全局作用域下的进行提升，因为此时函数中存储的都是字符串而已）

当代码执行遇到函数创建这个阶段的时候，直接跳过即可，因为在变量提升阶段已经执行了

私有作用域形成也不是立即执行代码，而是先进行变量提升（变量提升前，先形参赋值）

### 带var和不带var的区别

在全局作用域下声明一个变量，也相当于给window全局对象设置了一个属性，变量的值就是属性值（私有作用域下声明的私有变量和window没有关系）

#### `带var`

```javascript
console.log(a); //=>undefined
console.log(window.a); //=>undefined
console.log('a' in window); //=>true  在变量提升阶段在全局作用域中声明了一个变量，此时就已经把a当做属性赋值给了window了，只不过此时还没有给a赋值，默认值是undefined，  in：检测某个属性是否隶属于这个对象
var a = 12; //=>全局变量值修改，window的属性值也会修改
console.log(a); //=>全局变量a  12
console.log(window.a); //=>window的一个属性名a   12 
```

#### `不带var`

```javascript
console.log(a);  //=> 报错  a is not defined
console.log(window.a);  //=>undefined
console.log('a' in window);  //=>undefined
a = 12; //=>在这一步的时候浏览器会自动添加 > window.a = 12;  这个时候已经不是变量了，而是在window下添加了一个属性
console.log(a);  //=> 12
console.log(window.a);  //=> 12

// => 不加var的本质是window的属性 
```

#### 全局变量和window中的属性存在"映射机制"

#### 私有作用域中带var和不带var也有区别
- 带var的在私有作用域变量提升阶段，都声明为私有变量，和外界没有关系
- 不带var不是私有变量，会向它的上级作用域查找，看是否为上级的变量，不是，继续向上查找。一直找到window为止（我们把这种查找机制称为:"作用域链"），也就是我们在私有作用域中操作的这个非私有变量，是一直操作别人的

#### 思考题：
```javascript
console.log(a,b);
var a = 12,
    b = 12;

function fn() {
    console.log(a,b);
    var a = b = 13;
    console.log(a,b);
}
fn();
console.log(a,b);
```
### 变量提升机制下重名的处理

> 带var和function关键字声明相同的名字，这种也算是重名了（其实是一个fn，只是存储值的类型不一样）
>
> ```javascript
> var fn = 12;
> function fn (){
>     
> }
> ```

`重名的处理`

> 如果名字重复了，不会重新的声明，但是会重新的定义（更新赋值），不管是变量提升还是代码执行阶段皆是如此



### ES6中的let不存在变量提升

> 在ES6中基于let/const等方式创建变量或者函数，不存在变量提升机制
>
> ```javascript
> console.log(a);  //报错  Uncaught ReferenceError: Cannot access 'a' before initialization
> let a = 12;
> console.log(window.a);  //=>undefined
> ```
>
> 切断了全局变量和window属性的映射机制
>
> 虽然没有变量提升机制，但是在当前作用域代码自上而下执行之前，浏览器会做一个重复检测（语法检测）：自上而下查找当前作用域下所有变量，一旦发现有重复的，直接抛出异常，代码也不会在执行了（虽然没有把变量提升声明定义，但是浏览器已经记住了，当前作用域下有那些变量）

### ES6暂时性死区

```javascript
var a = 123;
if (true) {
    console.log(a); //=> 报错 Uncaught ReferenceError: Cannot access 'a' before initialization
    let a = 13;
    }
```






