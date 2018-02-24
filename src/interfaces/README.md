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

## REF

- [Interfaces - TypeScript][interfaces]

[interfaces]: https://www.typescriptlang.org/docs/handbook/interfaces.html