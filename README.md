# apple-development-process

- [apple-development-process](#apple-development-process)
  - [Preresearch](#preresearch)
    - [Apple devices by iOS version worldwide](#apple-devices-by-ios-version-worldwide)
    - [Apple devices by MacOS version worldwide](#apple-devices-by-macos-version-worldwide)
  - [Part 1 Swift](#part-1-swift)
  - [Part 2.1 SwiftUI](#part-21-swiftui)
    - [Animation](#animation)
  - [Part 2.2 SwiftUI And Combine](#part-22-swiftui-and-combine)
  - [Part 3 CoreData](#part-3-coredata)
  - [Part 4 iCloudKit](#part-4-icloudkit)
  - [Part 5 Multi-Platform | MacOS And IOS](#part-5-multi-platform--macos-and-ios)
  - [Part 6 AppStore Verify](#part-6-appstore-verify)
  - [Part \* SwiftUI With UIKit Or AppKit](#part--swiftui-with-uikit-or-appkit)
  - [Others](#others)
    - [how to get deviceId](#how-to-get-deviceid)


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
  
## Part 1 Swift

## Part 2.1 SwiftUI

**Some View** \
Some View类型的入参或返回值，不仅需要遵守View协议，并且需要确定为同一类型，这一点由编译器保证。\
例如一个返回值可能是 RounadRect 或 Rect 的函数就是不被允许的，虽然这两种类型都遵守View协议，但是函数最终的返回类型不是确定的。\
这里注意一个特例就是 var body : some View，由于body是被 @ViewBuilder 修饰过的，所以他的返回值其实是确定的 Tupe\<View\> 

### Animation

## Part 2.2 SwiftUI And Combine

## Part 3 CoreData

## Part 4 iCloudKit

## Part 5 Multi-Platform | MacOS And IOS

## Part 6 AppStore Verify

## Part * SwiftUI With UIKit Or AppKit

## Others
### how to get deviceId
https://stackoverflow.com/questions/19402327/how-to-get-unique-id-in-ios-device/32181411#32181411

