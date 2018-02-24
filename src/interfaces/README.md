# 接口

## 介绍

TS 的一个核心原则是，类型检查主要关注变量的“形状”，即常说的“鸭子类型”或“结构子类型”。TS 的 interfaces 充当命名这些类型的角色，可以定义代码的接口规范。

## 第一个 Interface

```js
function printLabel(labelledObj: { label: string }) {
    console.log(labelledObj.label)
}

let myObj = { size: 10, label: 'Size 10 Object' }
printLabel(myObj)
```

我们可以使用 interface 重写上面的代码：

```js
interface LabelledValue {
    label: string
}

function printLabel(labelledObj: LabelledValue) {
    console.log(labelledObj.label)
}

let myObj = { size: 10, label: 'Size 10 Object' }
printLabel(myObj)
```

我们可以使用 `LabelledValue` 接口来描述上一个案例中的需求。依然表示拥有一个属性，名为 `label`，字符串类型。注意，我们这里没有像其他语言那样，强制要求传入 `printLabel` 的对象必须实现这个 interface。

## 可选属性

不是所有的接口属性都是必需的。有些值只在特定情况下出现。在“选项袋子”模式下这种情形很常见。

下面是这种模式的一个例子：

```js
interface SquareConfig {
    color?: string;
    width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
    let newSquare = { color: 'white', area: 100 }    
    if (config.color) {
        newSquare.color = config.color
    }
    if (config.width) {
        newSquare.area = config.width * config.width
    }

    return newSquare
}

let mySquare = createSquare({ color: 'black' })
```

可选参数和普通参数一样，只是在最后多了一个 `?` 后缀。

## 只读属性

有些属性只能在创建时可编辑。可以在属性名之前增加 `readonly` 标识该属性只读：

```js
interface Point {
    readonly x: number;
    readonly y: number;
}

let p1: Point = { x: 10, y: 20 }
p1.x = 5 // error!
```

TypeScript 自带一个 `ReadonlyArray<T>` 类型，它和 `Array<T>` 相仿，只是移除了所有的更改属性的方法，因此你可以确保创建后数组的不可编辑：

```js
let a: number[] = [1, 2, 3, 4]
let ro: ReadonlyArray<number> = a

ro[0] = 12 // error!
ro.push(5) // error!
ro.length = 100 // error!
a = ro // error!
```

从最后一行的代码可以看到，即使将整个 `ReadonlyArray` 重新赋值给原数组也是违法的。但是可以通过类型断言覆盖它：

```js
a = ro as number[]
```

### `readonly` 和 `const`

何时使用 `readonly`，何时使用 `const`，最简单的判断方法就是问问自己，要把它用在变量，还是用在属性。变量使用 `const`，而属性使用 `readonly`。

## 多余属性检查

在第一个例子中，TypeScript 允许向形参格式为 `{ label: string; }` 的函数传递 `{ size: number; label: string; }` 的对象。我们也知道可选属性，它们在“选项袋子”模式中十分有效。

但是，将两者结合可能会让你自找麻烦，就像你在 JavaScript 中常遇到的那样。比如，以上个代码 `createSquare` 为例：

```js
interface SquareConfig {
    color?: string
    width?: number
}

function createSquare(config: SquareConfig): { color: string; area: number } {
    // ...
}

let mySquare = createSquare({ colour: 'red', width: 100 })
```

注意到传递给 `createSquare` 的参数拼成了 `colour`，而不是期望的 `color`。在原生 JavaScript 中，这种事情会悄悄失败。

但是，TypeScript 会认为这种情况是一种错误。对象字面量会受到特殊对待，当把它们赋值給其他变量，或者传递给函数时，就会经历所谓的“多余属性检查（*excess property checking*）”。如果对象字面量的属性无法在“目标类型”中找到，就会报错：

```js
// error: 'colour' not expected in type 'SquareConfig'
let mySquare = createSquare({ colour: 'red', width: 100 })
```

（未完待续）

## REF

- [Interfaces - TypeScript][interfaces]

[interfaces]: https://www.typescriptlang.org/docs/handbook/interfaces.html