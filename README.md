# ReactiveCocoa-CN-Guide
> [ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa) 中文文档翻译。目前由 [@没故事的卓同学](http://weibo.com/1926303682) 和 [@halffrost]( http://weibo.com/u/1936502837) 维护翻译。

![](https://github.com/ReactiveCocoa/ReactiveCocoa/raw/master/Logo/header.png)

#### Cocoa 框架的响应式扩展框架，基于 [ReactiveSwift][]

⚠️ [Objective-C 版本](https://github.com/ReactiveCocoa/ReactiveObjC) ⚠️ [ Swift 2.x 版本](https://github.com/ReactiveCocoa/ReactiveCocoa/tree/v4.0.0)

## ReactiveSwift 是什么
__ReactiveSwift__ 为处理___随着时间传递的数据流___ 提供了可组合、声明式和灵活的基本元素（primitives）。这些基本元素用于统一表示基于观察的常见 Cocoa 和通用编程模式。

更多关于核心基本元素的信息，查看 [ReactiveSwift][] 。

## ReactiveCocoa 是什么

__ReactiveCocoa__ 通过使用声明式的 [ReactiveSwift][] 基本元素包含了 Cocoa 框架的多个方面的编程模式。 

1. **绑定 UI**

   UI 组件公开 [`BindingTarget`][] ，通过 `<~` 操作符绑定任意类型数据流的值。

   ```swift
   // 把 `person` 的 `name` 属性绑定到 `UILabel` 的文字上
   nameLabel.text <~ person.name
   ```

2. **控件和用户的交互**

   可交互的 UI 组件通过公开 [`Signal`][] 用于事件响应和通过用户的交互更新控件的值。

   为一些控件绑定 [`Action`][] 提供了方便快捷的 API 。

   ```swift
   // 当 toggle 的被点击时 `allowsCookies` 的值就会更新
   preferences.allowsCookies <~ toggle.reactive.isOnValues 

   // 根据 textField 中用户的输入实时输出当前文本的字符长度
   textField.reactive.continuousTextValues.map { $0.characters.count }

   // 当按钮被点击时触发 `commit` 
   button.reactive.pressed = CocoaAction(viewModel.commit)
   ```

3. **声明式的 Objective-C 动态机制**

   通过拦截 Objective-C 对象生成信号，
   比如下面例子中方法调用的拦截和对象释放。

   ```swift
   // 每次在 `viewWillAppear(_:)` 调用后都会被通知
   let appearing = view.reactive.trigger(for: #selector(viewWillAppear(_:)))

   // 观察 `object` 的生命周期
   object.reactive.lifetime.ended.observeCompleted(doCleanup)
   ```

4. **易读、 安全的 KVO**

   把 KVO 机制用 [`SignalProducer`][]  和 `DynamicProperty` 的形式表示，可以方便的继承合成。

   ```swift
   // 当对象 `keyPath` 的值变化时发送这个值的 producer
   //
   // 当 `self` 的生命周期结束时停止 KVO
   let producer = object.reactive.values(forKeyPath: #keyPath(key))
   	.take(during: self.reactive.lifetime)

   // property 是 person 对应的 keyPath 的值。对这个对象是弱引用(weak reference)。 
   let property = DynamicProperty<String>(object: person,
                                          keyPath: #keyPath(person.name))
   ```

But there are still more to be discovered and introduced. Read our in-code documentations and release notes to find out more.

## Getting started

ReactiveCocoa supports macOS 10.9+, iOS 8.0+, watchOS 2.0+, and tvOS 9.0+.

#### Carthage

If you use [Carthage][] to manage your dependencies, simply add
ReactiveCocoa to your `Cartfile`:

```
github "ReactiveCocoa/ReactiveCocoa"
```

If you use Carthage to build your dependencies, make sure you have added `ReactiveCocoa.framework`, `ReactiveSwift.framework`, and `Result.framework` to the "_Linked Frameworks and Libraries_" section of your target, and have included them in your Carthage framework copying build phase.

#### CocoaPods

If you use [CocoaPods][] to manage your dependencies, simply add
ReactiveCocoa to your `Podfile`:

```
pod 'ReactiveCocoa', :git => 'https://github.com/ReactiveCocoa/ReactiveCocoa.git'
```

#### Git submodule

 1. Add the ReactiveCocoa repository as a [submodule][] of your
    application’s repository.
 2. Run `git submodule update --init --recursive` from within the ReactiveCocoa folder.
 3. Drag and drop `ReactiveCocoa.xcodeproj`,
    `Carthage/Checkouts/ReactiveSwift/ReactiveSwift.xcodeproj`, and
    `Carthage/Checkouts/Result/Result.xcodeproj` into your application’s Xcode
    project or workspace.
 4. On the “General” tab of your application target’s settings, add
    `ReactiveCocoa.framework`, `ReactiveSwift.framework`, and `Result.framework`
    to the “Embedded Binaries” section.
 5. If your application target does not contain Swift code at all, you should also
    set the `EMBEDDED_CONTENT_CONTAINS_SWIFT` build setting to “Yes”.

## Have a question?
If you need any help, please visit our [GitHub issues][] or [Stack Overflow][]. Feel free to file an issue if you do not manage to find any solution from the archives.

[ReactiveSwift]: https://github.com/ReactiveCocoa/ReactiveSwift
[ReactiveObjC]: https://github.com/ReactiveCocoa/ReactiveObjC
[GitHub issues]: https://github.com/ReactiveCocoa/ReactiveCocoa/issues?q=is%3Aissue+label%3Aquestion+
[Stack Overflow]: http://stackoverflow.com/questions/tagged/reactive-cocoa
[CHANGELOG]: CHANGELOG.md
[Carthage]: https://github.com/Carthage/Carthage
[CocoaPods]: https://cocoapods.org/
[submodule]: https://git-scm.com/book/en/v2/Git-Tools-Submodules
[`Signal`]: https://github.com/ReactiveCocoa/ReactiveSwift/blob/master/Documentation/FrameworkOverview.md#signals
[`SignalProducer`]: https://github.com/ReactiveCocoa/ReactiveSwift/blob/master/Documentation/FrameworkOverview.md#signal-producers
[`Action`]: https://github.com/ReactiveCocoa/ReactiveSwift/blob/master/Documentation/FrameworkOverview.md#actions
[`BindingTarget`]: https://github.com/ReactiveCocoa/ReactiveSwift/blob/master/Documentation/FrameworkOverview.md#binding-target