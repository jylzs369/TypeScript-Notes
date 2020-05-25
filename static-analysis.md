## 静态检查

静态检查是不执行代码的情况下，进行代码分析，排查错误的方式。

`TypeScript` 静态分析基于代码结构（隐式）和类型注释（显式）。

`boolean`、`number`、`string`、`null`、`undefined`、`symbol` 在 `TypeScript` 代表 `JS` 中的基础数据类型，而 `Boolean`、`Number`、`String`、`Symbol` 是构造函数。

`Any` 类型用于放弃对未知类型数据的静态分析，使用其注释的数据将在编译时通过检查。

初次定义变量的时候未指定类型也没有赋值，那么 `TypeScript` 会将此变量推断成 `Any` 类型。初次定义变量时未指定类型但赋值了，`TypeScript` 会将赋予值的数据类型推断出指定给变量。

对于对象的类型使用接口（Interfaces）来定义

`Object` 类型允许赋给变量任何类型的值，但不能随意调用方法。原理是 `JavaScript` 中任意数据类型的值都可以被包装为对象，所以不管实际赋的值是哪种数据类型，其本身具有的方法并不在对象类型中拥有，所以那些方法不能被正确调用。通常避免使用非基本数据类型的类型注释。

使用只读 `readonly` 约束接口属性时，在整个对象第一次定义的时候就生效。


**类型断言**：

使用联合类型注释的参数或变量，只有调用各类型共有的方法才能通过检查，为了使检查正确通过，可以使用类型断言。

```
// 报错
function getLength (value: string | number): number {
  if (value.length) { // value可以是string也可是number，但number类型不具有length属性，所以检查器提示：类型“number”上不存在属性“length”。
    return value.length
  } else {
    return value.toString().length
  } 
}

// 修改过后的
function getLength (value: string | number): number {
  if ((value as string).length) { // 显式选择将value作为string类型来处理，检查器会忽略对number类型的检查，而由于value实际没有length属性，条件语句判断为否然后进入else语句，此处可以证明类型断言并没有将value进行类型转换。
    return (value as string).length // 此处的断言不能省略为value.length，因为还是会执行对number类型的属性检查
  } else {
    return value.toString().length
  } 
}
```

## 类
类属性声明中，设置的属性默认为共有属性，私有属性需要使用关键字 `private`。

```
class Person {
  fullName: string
}
```

类的构造函数中，在参数前使用关键字 `public`，会自动创建对象的相应属性。

```
class Person {
  constructor (public firstName: string, public lastName: string) {
    this.fullName = firstName + lastName
  }
}
```