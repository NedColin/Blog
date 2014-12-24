---
layout:     post
title:      Swift 一些常见关键字
date:       2014-06-11 15:31:19
summary:    @lazy convenience mutating typealias.
categories: jekyll pixyll
---

@lazy

延迟存储属性是指当第一次被调用的时候才会计算其初始值的属性。在属性声明前使用@lazy来标示一个延迟存储属性。

注意：必须将延迟存储属性声明成变量（使用var关键字），因为属性的值在实例构造完成之前可能无法得到。而常量属性在构造过程完成之前必须要有初始值，因此无法声明成延迟属性。

延迟属性很有用，当属性的值依赖于在实例的构造过程结束前无法知道具体值的外部因素时，或者当属性的值需要复杂或大量计算时，可以只在需要的时候来计算它。

convenience
lass中负责初始化所有properties、调用super.init的init方法都是Designated Initializers. 比如上面的init方法都是。 Designated Initializers中不能调用其他init方法，只能必须（如果有super）调用super.init 
有时候我们需要通过不同的方式init，但是又想重用部分代码，这里引入了convenience关键字来修饰init，实现Convenience Initializers。 比如
  convenience init(p:Person) {
      self.init(name:p.name, age:p.age)
  }
Convenience Initializers内部需要遵循以下规则以保证super.init只调用1次
可以调用某个Convenience Initializer或者Designated Initializer，注意只能调用1次。
绝对不能调用super.init
必须保证Designated Initializer直接或者间接的被调用1次

mutating

在 swift 中，包含三种类型(type): structure , enumeration , class

其中structure和enumeration是值类型( value type ),class是引用类型( reference type )

但是与objective-c不同的是，structure和enumeration也可以拥有方法(method)，其中方法可以为实例方法(instance method)，也可以为类方法(type method)，实例方法是和类型的一个实例绑定的。

在swift官方教程中有这样一句话：

“Structures and enumerations are value types.
 By default, the properties of a value type cannot be modified from within its instance methods.”

摘录来自: Apple Inc. “The Swift Programming Language”。 iBooks. https://itunes.apple.com/WebObjects/MZStore.woa/wa/viewBook?id=881256329
大致意思就是说，虽然结构体和枚举可以定义自己的方法，但是默认情况下，实例方法中是不可以修改值类型的属性。

举个简单的例子，假如定义一个点结构体，该结构体有一个修改点位置的实例方法：

struct Point {  var x = 0, y = 0      func moveXBy(x:Int,yBy y:Int) {    self.x += x    // Cannot invoke '+=' with an argument list of type '(Int, Int)'     self.y += y    // Cannot invoke '+=' with an argument list of type '(Int, Int)'   }
}
编译器抛出错误，说明确实不能在实例方法中修改属性值。

为了能够在实例方法中修改属性值，可以在方法定义前添加关键字 mutating

struct Point {  var x = 0, y = 0      mutating func moveXBy(x:Int,yBy y:Int) {    self.x += x    self.y += y  }
}var p = Point(x: 5, y: 5)

p.moveXBy(3, yBy: 3)
另外，在值类型的实例方法中，也可以直接修改self属性值。

enum TriStateSwitch {  case Off, Low, High  mutating func next() {    switch self {    case Off:      self = Low    case Low:      self = High    case High:      self = Off    }  }
}var ovenLight = TriStateSwitch.Low
ovenLight.next()// ovenLight is now equal to .High ovenLight.next()// ovenLight is now equal to .Off”
TriStateSwitch枚举定义了一个三个状态的开关，在next实例方法中动态改变self属性的值。

当然，在引用类型中(即class)中的方法默认情况下就可以修改属性值，不存在以上问题。

typealias

类型别名(type aliases)就是给现有类型定义另一个名字。你可以使用typealias关键字来定义类型别名。

当你想要给现有类型起一个更有意义的名字时，类型别名非常有用。假设你正在处理特定长度的外部资源的数据：

typealias AudioSample = UInt16
定义了一个类型别名之后，你可以在任何使用原始名的地方使用别名：

var maxAmplitudeFound = AudioSample.min
// maxAmplitudeFound 现在是 0
本例中，AudioSample被定义为UInt16的一个别名。因为它是别名，AudioSample.min实际上是UInt16.min，所以会给maxAmplitudeFound赋一个初值0。

<blockquote>
  <p>
    Perfection is achieved, not when there is nothing more to add, but when there is nothing left to take away.
  </p>
  <footer><cite title="Antoine de Saint-Exupéry">Antoine de Saint-Exupéry</cite></footer>
</blockquote>

## Where is it?

Checkout the [Github repository](https://github.com/johnotander/pixyll) to download it, request a feature, or report a bug.

It's free, and open source ([MIT](http://opensource.org/licenses/MIT)).
