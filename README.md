# apple-development-process

- [apple-development-process](#apple-development-process)
  - [Preresearch](#preresearch)
    - [Apple devices by iOS version worldwide](#apple-devices-by-ios-version-worldwide)
    - [Apple devices by MacOS version worldwide](#apple-devices-by-macos-version-worldwide)
  - [Part 1.1 Swift](#part-11-swift)
    - [struct vs class](#struct-vs-class)
    - [@escaping](#escaping)
    - [对比 some、any、generic](#对比-someanygeneric)
  - [Part 1.2 Swift Networking library: Alamofire](#part-12-swift-networking-library-alamofire)
  - [Part 2.1 SwiftUI](#part-21-swiftui)
    - [App启动](#app启动)
    - [Animation](#animation)
    - [Transition 控制视图appear和disappear动画](#transition-控制视图appear和disappear动画)
  - [Part 2.2 SwiftUI And Combine](#part-22-swiftui-and-combine)
  - [Part 3 CoreData](#part-3-coredata)
  - [Part 4 iCloudKit](#part-4-icloudkit)
  - [Part 5 Multi-Platform | MacOS And IOS](#part-5-multi-platform--macos-and-ios)
  - [Part 6 AppStore Verify](#part-6-appstore-verify)
  - [Part \* SwiftUI With UIKit Or AppKit](#part--swiftui-with-uikit-or-appkit)
  - [Others](#others)
    - [how to get deviceId](#how-to-get-deviceid)
    - [get SIL or llvm-IR](#get-sil-or-llvm-ir)


## Preresearch
### Apple devices by iOS version worldwide
<img width="783" alt="截屏2024-03-01 19 30 12" src="https://github.com/AsilenceBTF/apple-development-process/assets/51771808/a4fe665e-8bab-46c3-82f8-54b0a96520b5">

- iOS 17: 63.2%, up from 38.92% last month
- iOS 16: 33.0%, down from 57.07,% last month
- iOS 15: 3.7%
- iOS 14: 0.06%
- Other

### Apple devices by MacOS version worldwide
- MacOS 14: 73%
- MacOS 13: 18%
- MacOS 12: 6%
- MacOS 11: 1%
  
## Part 1.1 Swift

### struct vs class
先说结论: 在swift和swiftUI中优先使用struct.  
swift中的struct和class一样，可以包含:
- properties 特性
- methods 方法
- subscripts 下标
- initializers 初始化器
- protocol conformances 协议一致性
- extensions 扩展

**不同点:**  
- **mutation**  
  struct是不可变的，如果要在方法里修改struct，需要添加mutable标记。
- **引用类型和值类型**  
  - **class是引用类型**。引用类型变量存储了对数据内存的引用，如果我们有几个相同数据的引用，他们就像是指向同一个数据内存的指针。因此如果我们通过其中一个改变数据，其他引用获取的数据也会改变。  
  - **struct是值类型**。值类型和引用类型相反，每个变量都会创建自己的副本，其中一个变化对其他变量都不会有影响。
- **内存管理**  
  - **struct** struct内存在栈上分配，因此也是线程安全的，所需的内存量不需要动态计算，在编译期间就已经计算好，所以使用起来比堆内存快得多。  
  - **class** 在堆上分配，理论上全局都可以访问，因此不是线程安全的，需要动态计算内存。  
  - **特殊情况**
    - **class内包含struct**  显而意见这个时候的struct的生命周期需要和class一致，你肯定不希望class正在使用结果struct被释放了，所以struct和class一样保存在堆内存中。
    - **struct内包含class**  这种情况swift提供了一种解决方法，copy-on-write。
      - **copy-on-write** 写入时复制是一种内存优化，当对struct进行复制时并不会创造新值，只有当修改第二个值的时候，才会进行复制。这个优化目前只针对某些特殊的swift结构(例如Array、Set和Dict), 一般包含引用的struct并不适用。
      - **关联引用计数** 如果你想优化这种情况下的内存管理，可以实现自己的写时复制结构，[实现地址](https://github.com/apple/swift/blob/main/docs/OptimizationTips.rst#advice-use-copy-on-write-semantics-for-large-values)。

### @escaping
@escaping用来修饰入参的闭包，`当闭包的生命周期超过当前函数的生命周期时`，我们需要使用该关键字标识。  
以下情况会导致闭包的生命周期超过当前函数的生命周期：
- 异步执行的闭包
```swift
func doAsySomething(completion: @escaping () -> ()) {
    print("begin - doAsySomething")
    DispatchQueue.main.async {
        completion()
    }
    print("end - doAsySomething")
}

```
- 无法确定何时执行
```swift
var completions: [()->()]!

func doSomething(completion: @escaping () -> ()) {
    completions.append(completion)
}

```

### 对比 some、any、generic


## Part 1.2 Swift Networking library: Alamofire


## Part 2.1 SwiftUI

### App启动
```swift
@main
struct ExampleApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```
- **@main**  
  main标签修饰当前struct作为application的入口
- **ExampleApp**  
  ExampleApp遵守App协议，提供应用启动的视图
- **var body: some Scene**   
  App协议要求提供一个body属性，body本身遵守Scene协议
- **some**  
  有关some关键字的细节请阅读Part1
- **Scene**  
  Scene是视图层次结构的容器，遵守Scene的body描述了当前展示给用户的视图结构。系统会根据当前app的状态来判断视图结构呈现的方式和时机。  
  这在不同平台上呈现效果可能不一样，例如在iOS上屏幕一次只显示一个Scene，但是在MacOS上每个窗口都可能是一个Scene。
- **WindowGroup**  
  WindowGroup用来管理窗口集合，一般应用在跨平台场景。

**Some View**  
Some View类型的入参或返回值，不仅需要遵守View协议，并且需要确定为同一类型，这一点由编译器保证。  
例如一个返回值可能是 RounadRect 或 Rect 的函数就是不被允许的，虽然这两种类型都遵守View协议，但是函数最终的返回类型不是确定的。  
这里注意一个特例就是 var body : some View，由于body是被 @ViewBuilder 修饰过的，所以他的返回值其实是确定的 Tupe\<View\> 

### Animation
swift的动画从大类可以分为显示动画和隐式动画，具体有以下几种场景触发动画
- withAnimation()
- .onAppear
- .transition
- .animation (隐式动画)

在大多数场景下，使用除了.animation之外的显示动画是更优选择。

**withAnimation**


### Transition 控制视图appear和disappear动画



## Part 2.2 SwiftUI And Combine

## Part 3 CoreData

## Part 4 iCloudKit

## Part 5 Multi-Platform | MacOS And IOS

## Part 6 AppStore Verify

## Part * SwiftUI With UIKit Or AppKit

## Others
### how to get deviceId
https://stackoverflow.com/questions/19402327/how-to-get-unique-id-in-ios-device/32181411#32181411
### get SIL or llvm-IR
`swiftc -emit-sil main.swift`  
`swiftc -emit-ir main.swift`


