# A simple project to render a UI Layout in SwiftUI getting the rendering time to compare against UIKit

The layout consist in a simple ScrollView having a nested VStack containing 100 rows.
Each row has its own layout consisting in a HStack containing an Image and a Text.

No List, LazyVStack or any other optimization is implemented because the purpose is to stress the system forcing the render of all Views happens before the `onAppear`.

This test project has been implemented to do an high level comparison rendering a simple UI in SwiftUI.
There is a companion project in which the same layout has been implemented in UIKit trying to replicate the same View hierarchy:
https://github.com/maxbellucci69/TestUIKit

The time to render is calculated on the onAppear for SwiftUI and viewDidAppear for UIKit.
A start time retrieved with `CFAbsoluteTimeGetCurrent()` is stored at initialization of properties happening on creation of the View for SwiftUI, similarly it is stored at UIViewController properties initialization for UIKit.
The elapsed time is calculate as the difference between the `CFAbsoluteTimeGetCurrent()` value read when the view is appear.

You can increase the number of rows generated simply changing the upper bound in the ForEach, but take care that the rendering time will increase dramatically in UIKit:

## Values measured on physical device iPhone 7 - 128GB - iOS 15.8.2
Project built as Release target with optimization and directly deployed on the device.

| Number of rows | Rendering time SwiftUI | Rendering time UIKit |
| --- | --- | --- |
| 100 rows | 0.056s | 0.14s | 
| 500 rows | 0.2s | 4.0s | 
| 1000 rows | 0.33s | 29.34s | *** The rendering time for UIKit is so long that Apple close the app if you don't build and run directly from XCode ***

