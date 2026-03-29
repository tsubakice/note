# TypeScript 装饰器

> 下文均使用 TS 指代 TypeScript，JS 指代 JavaScript（全称太长懒得打）

## 1. 简介

- 装饰器本质是一种特殊的 **函数**，它可以对：类、属性、方法、参数进行扩展，同时能让代码更简洁。
- 装饰器自 **2015** 年 在 **ECMAScript-6** 中被提出到现在，已经 10 年了。
- 截至目前，装饰器依然是 **实验性特性**，需要开发者手动调整配置，来开启装饰器支持。
- 装饰器有 5 种：类装饰器、属性装饰器、方法装饰器、访问器装饰器、参数装饰器。

> 备注：虽然 **TS5.0** 中可以直接使用 **类装饰器**，但为了确保其他装饰器可用，现阶段使用时，仍建议使用 **experimentalDecorators** 选项来开启装饰器支持，而且不排除在未来版本中，官方会进一步调整装饰器的相关语法。

## 2. 类装饰器

### 2.1. 基本语法

> 类装饰器是一种应用在 **类声明** 上的 **函数**，可以为类添加额外的功能，或添加额外的逻辑。

```ts
/**
 * 函数 Test 会在实例化被装饰的类时执行
 * @param target 被装饰的类
 * @constructor
 */
const Test = (target: Function) => {
  console.log(target)
}

@Test
class Person {
}
```

### 2.2. 应用举例

> 需求：定义一个装饰器，实现 Person 实例调用 toString 方法时，返回 JSON.stringify 的执行结果。

```ts
const Test = (target: Function) => {
  target.prototype.toString = function (): string { // 注意这里因为用到了 this，所以不能用箭头函数哦
    return JSON.stringify(this)
  }
}

@Test
class Person {
  constructor(public name: string) {
  }
}

console.log(new Person("John Doe").toString())
```

### 2.3. 关于返回值

> 类装饰器 **有返回值**：若类装饰器返回一个新的类，那么这个新的类将替换掉被装饰的类。
>
> 类装饰器 **无返回值**：若类装饰器为返回一个新的类，那么被装饰的类则不会被替换。

```ts
const Test = (target: Function) => class {
  test() {
    console.log(100, 200, 300)
  }
}

@Test
class Person {
  test() {
    console.log(100)
  }
}

new Person().test() // 执行结果是 100 200 300
```

### 2.4. 关于构造类型

> 在 TS 中，Function 类型所表示的范围十分广泛，包括普通函数、箭头函数、方法等等。但并非 Function 类型的函数都可以被 new 关键字实例化，例如箭头函数是不能被实例化的，那么 TS 中该如何声明一个构造类型呢？有如下两种方式：

```ts
/**
 * 定义构造函数类型
 * new：表示构造函数调用
 * ...args: any[]：表示任意数量的任意类型参数
 * {}：表示对象
 */
type Constructor = new (...args: any[]) => {};

const test = (func: Constructor) => {}

class Person {
}

test(Person) // 不爆错
test(() => {}) // 爆错
```

```ts
// 定义一个包含静态属性 wife 的构造类型
type Constructor = {
  new(...args: any[]): {},
  wife: string, // 自定义静态属性
}

class Person {
  static wife: string
}

const test = (fn: Constructor) => {}

test(Person) // 不爆错
test(class {}) // 爆错
```

### 2.5. 替换被装饰的类

对于高级一些的装饰器，不仅仅是覆盖原型上的一个方法，还要有更多功能，例如添加新的方法和状态。

> 需求：设计一个装饰器，可以给实例添加一个属性，用于记录实例对象的创建时间，再添加一个方法用于读取创建时间。

```ts
type Constructor = new (...args: any[]) => {};

const Test = <T extends Constructor>(target: T) => class extends target {
  private readonly timestamp: Date
  constructor(...args: any[]) {
    super(...args)
    this.timestamp = new Date();
  }

  current() {
    console.log(this.timestamp);
  }
}

@Test
class Person {
  constructor(public name: string) {
  }
}

interface Person { // 防止爆错用的
  current: () => void,
}

new Person('tsubaki').current()
```

## 3. 装饰器工厂

装饰器工厂是一个返回装饰器函数的函数，可以为装饰器添加参数，可以更灵活的控制装饰器的行为。

> 需求：定义一个装饰器工厂，实现 Person 实例可以调用到 introduce 方法，且 introduce 中输出内容的次数由装饰器工厂接收的参数决定。

```ts
type Constructor = new (...args: any[]) => {};

const Test = (n: number) => ((target: Constructor) => {
  target.prototype.introduce = () => {
    for (let i = 0; i < n; i++) {
      console.log('hello world')
    }
  }
})

@Test(5)
class Person {
  constructor(public name: string) {
  }
}

interface Person { // 防止爆错用的
  introduce: () => void
}

new Person('tsubaki').introduce()
```

## 4. 装饰器组合

装饰器可以组合使用，执行顺序为：先 **由上到下** 执行 **所有装饰器工厂**，依次获取到装饰器，然后再 **由下到上** 执行 **所有装饰器**。

```ts
const Test1 = (target: Function) => console.log('Test1')
const Test2 = () => {
  console.log('Test2 工厂')
  return (target: Function) => {
    console.log('Test2')
  }
}
const Test3 = () => {
  console.log('Test3 工厂')
  return (target: Function) => {
    console.log('Test3')
  }
}
const Test4 = (target: Function) => console.log('Test4')

@Test1
@Test2()
@Test3()
@Test4
class Person {
}
```

## 5. 属性装饰器

### 5.1. 基本语法

```ts
/**
 * 属性装饰器
 * @param target 对实例属性来说值是类的原型对象，对静态属性来说值是属性所属的类
 * @param key 属性名
 * @constructor
 */
const Test = (target: object, key: string) => console.log(target, key)

class Student {
  @Test name: string
  @Test static school: string

  constructor(name: string) {
    this.name = name
  }
}
```

### 5.2. 关于属性遮蔽

> 观察下述代码：猜猜 `const person = new Person('tsubaki', 21)` 这行代码写在 1 号位置和 2 号位置会有什么不一样么？

```ts
class Person {
  constructor(public name: string, public age: number) {
  }
}

// const person = new Person('tsubaki', 21) // 1 号位置

let init = 18
Object.defineProperty(Person.prototype, 'age', {
  get: (): number => init,
  set: (value: number) => {
    init = value
  }
})

// const person = new Person('tsubaki', 21) // 2 号位置

console.log(person.age, Person.prototype.age)
```

答案揭晓：当这行代码写在 2 号位置时，会造成 **属性遮蔽** 这一异常现象，而写在 1 号位置则不会造成什么异常现象。

问题就出在 JS（由于 TS 最终会被编译为 JS，所以这其实是 JS 的坑）的属性创建机制上，当执行 `this.age = age` 这类代码时，JS 会先查找对象本身是否具有 age 属性，若有则正常赋值，不会重复创建 age 属性；若没有则会 **顺着原型链一层一层向上查找**，找到最后都没找到，才会在对象身上创建 age 属性。这也就解释了为什么 `const person = new Person('tsubaki', 21)` 这行代码写在 2 号位置时会造成 **属性遮蔽** 这一异常现象，因为在调用 Person 的构造函数执行 `this.age = age` 这行代码前，已经在 Person.prototype 上定义了 age 属性，能在原型链上找到 age 这一属性，那么就不会为 Person 实例创建 age 属性，而是复用 Person.prototype 身上的 age 属性，所以最后 Person.prototype.age 的值才会是 21 而非 18。

### 5.3. 应用举例

> 需求：定义一个属性装饰器，用于监视属性的修改。

```ts
const Test = (target: object, key: string) => {
  const __key = `__${key}`
  Object.defineProperty(target, key, {
    get() {
      return this[__key]
    },
    set(value) {
      this[__key] = value
      console.log(key + ' 属性的最新值是 ' + value)
    },
  })
}

class Person {
  @Test name: string
  @Test age: number

  constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }
}

new Person('tsubaki', 21)
```

## 6. 方法装饰器

### 6.1. 基本语法

```ts
/**
 * 方法装饰器
 * @param target 对实例属性来说值是类的原型对象，对静态属性来说值是属性所属的类
 * @param key 属性名，贴切点就是方法名
 * @param descriptor 方法描述对象，其 value 属性是被装饰的方法
 * @constructor
 */
const Test = (target: object, key: string, descriptor: PropertyDescriptor) => {
  console.log(target, key, descriptor)
}

class Person {

  @Test
  speak() {
    console.log('hello hello')
  }

  @Test
  static isPerson(): boolean {
    return true
  }
}
```

### 6.2. 应用举例

> 需求：定义一个方法装饰器，用于在方法执行前和执行后，均追加一些额外逻辑。

```ts
const Test = (target: object, key: string, descriptor: PropertyDescriptor) => {
  const origin = descriptor.value
  descriptor.value = function (...args: any[]) {
    console.log(key, '开始执行...')
    const result = origin.call(this, ...args)
    console.log(key, '结束执行...')
    return result
  }
}

class Person {

  @Test
  speak(message: string) {
    console.log('speak: ' + message)
  }
}

new Person().speak('hhh')
```

## 7. 访问器装饰器

### 7.1. 基本语法

```ts
/**
 * 方法装饰器
 * @param target 对实例访问器来说值是类的原型对象，对静态访问器来说值是属性所属的类
 * @param key 属性名，贴切点就是方法名
 * @param descriptor 方法描述对象，其 get 和 set 属性分别是 get 和 set 访问器
 * @constructor
 */
const Test = (target: object, key: string, descriptor: PropertyDescriptor) => {
  console.log(target, key, descriptor)
}

class Person {

  @Test
  get address() {
    return 'address'
  }

  @Test
  static get country() {
    return 'country'
  }
}
```

### 7.2. 应用举例

> 需求：对 Weather 类的 temp 属性的 set 访问器进行限制，设置最低温度 -50，最高温度 50。

```ts
const Test = (min: number, max: number) => ((target: object, key: string, descriptor: PropertyDescriptor) => {
  const originalSetter = descriptor.set!
  descriptor.set = function (value: number) {
    if (value < min || value > max) {
      throw new Error(`传入值 ${value} 必须大于 ${min} 或小于 ${max}`)
    }
    originalSetter(value)
  }
})

class Weather {
  constructor(private _temp: number) {
  }

  @Test(-50, 50)
  set temp(temp: number) {
    this._temp = temp
  }

  get temp() {
    return this._temp
  }
}

const w = new Weather(25)
w.temp = 90
console.log(w.temp)
```

## 8. 参数装饰器

### 8.1. 基本语法

```ts
/**
 * 参数装饰器
 * @param target 对实例方法参数来说值是类的原型对象，对静态方法参数来说值是方法所属的类
 * @param key 属性名，贴切点就是参数所在方法名
 * @param index 参数索引
 * @constructor
 */
const Test = (target: object, key: string, index: number) => {
  console.log(target, key, index)
}

class Person {
  speak(@Test message: string) {
    console.log('say: ' + message)
  }
}
```

### 8.2. 应用举例

> 需求：定义方法装饰器搭配参数装饰器，来对 speak 方法的参数类型进行限制。