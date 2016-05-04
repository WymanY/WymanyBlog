---
layout: post
title: 'iOS9 Day-by-Day :: Day 2 :: UI Testing'
date: '2015-12-30 20:28'
tags:
  - ios9 Day-by Day
  - UI Testing
---
>本文翻译自[www.shinobicontrols.com](www.shinobicontrols.com)官网 ios9-day-by-day系列，原作者是Chris Grant，本文是第二课，主要讲解了UI Testing相关的知识。

![ios9_cover](http://7xl8rl.com1.z0.glb.clouddn.com/ios9_cover.jpg)
<!-- more -->

对于开发一个软件应用，自动化界面测试是一个十分有价值的工具。它可以快速的检测你app 中出现的问题。一套成功的测试运行可以给你在发布正式版本前充足的自信。在 iOS 平台上，目前是通过`UIAutomation`来完成上述的工作，但是UIAutomation要求我们用 JavaScript 来写测试脚本。这涉及到打开一个单独的 app,instrument，并且要创建并运行一些脚本。这套工作流令人难以忍受的慢，而且需要花费很多时间来习惯。

### UI Testing

伴随着 Xcode7 的发布，苹果引进了一种新的方式来指导用户进行他们的界面测试。UI testing 允许用户找到UI 元素，并且可以交互，验证他们的属性和状态。UI testing 完全被一直到 Xcode7 的测试报告内，并且随着你的单元测试一块运行。XCTest 从 Xcode5时代就被苹果一直到它的测试框架内，但是从 Xcode7 开始，它被更新添加了 UI testing 能力。这允许你添加一些断言去检测特定时刻你的 UI 的状态。

### Accessibility

为了使得你的 UI Testing 能够运行，该框架需要能够访问内置UI 的各种元素，以便于它可以模范贯彻它的行为。当测试需要 tap 和 swipe 的时候，你可以定义一些指定的点，但是当处于不同尺寸的设备的时候或者你准备调整 UI 内置元素的位置的时候，它可能能会失败。

这就是 `accessibility`可以起到作用的地方。苹果 framework  很早就有的`Accessibility`提供了禁止用户与 app 交互的方法。关于你的 UI，它提供了丰富的语义学数据，对于受禁的用户，将给予不同的访问特性去打开你的 app。很多来自黑盒的accessibility只能在 app 内起作用，但是你可以（也应该）改善那些，有关于你UI的使用 `Accessibility`的接口的数据。在很多情况下这都十分的有必要，例如在自定义控件时，你`Accessibility`并不能搞懂你的接口做了自动化相关的什么。

UI Testing 通过你的应用的提供的`Accessibility`特性有分解你得应用的界面的能力，它有助于解决不同尺寸设备的问题，在你重新排布你的 UI 中的一些元素时，提供给你避免重新重写全套测试。实现`Accessibility`不仅在 UI 测试测试上十分有用，而且给你的 app 添加了使得受限用户使用的能力。(PS：这里的受限用户是指身体有残障的，如阅读障碍，视力有问题的人)

### UI Recording

一旦在你的 app 中设置了accessible的 UI，你就会想要创建一些 UI测试。写 UI 测试可能十分枯燥，浪费时间。如果你有一个负载的 UI 界面，就会更难。Apple 引进了 UI Recording，能够让你创建一些的测试，并且还可以扩展一些测试。相对的，在你与你设备或者模拟器中的 app交互的时候，就会自动生成相关代码。既然我们已经有了一个UI Testing 的搭配的概览了，是时候创建 它了。

### Creating UI Tests

我们准备使用最新的 UI 测试工具来创建一个 UI 测试体系去演示它如何工作的。如果你一步步的跟着完成，并看到结果，完整的 Demo 应用可以在 [Github](https://github.com/shinobicontrols/iOS9-day-by-day/tree/master/02-User-Interface-Testing) 中下载。

### Setup

在 Xcode7 中，当你创建一个新的项目的时候，你可以选择是否包含 UI Tests. 这将会为你所需要的全部配置设置一个 UI test 的 Target 的占位符。

![newProject](http://7xl8rl.com1.z0.glb.clouddn.com/newProject.png)

在这个实例中的项目十分简单，但是对于我们来说，足够能演示 Xcode7的 UI Testing 如何工作的。

![appLayout](http://7xl8rl.com1.z0.glb.clouddn.com/appLayout.png)

有一个叫**Menu**视图控制器， 它包含了一个`Switch`，一个`Button`，链接到一个详情视图控制器中。当 Switch 处于 "off"的状态时，button应该处于禁用的状态，导航也会被禁止。详情视图控制器包含一个简单的 Button，这个按钮可以增加下面 label 的数值。

### Using UI Recording

一旦 UI 和它的功能都被设置完毕，我们可以写一些 UI 测试去确保任何代码上的改变不会起到任何效果的改变。

### The XCTest UI Testing API

在我们开始记录一些动作之前，我们必须决定我们想要 Assert 的东西。为了 assert关于 UI相应的事件，我们可以使用`XCTest`框架，而且现在新引入了三个 API：

- **XCUIApplication**.它作为是你当前测试的应用的代理。它允许你启动你的应用以便于你可以运行测试。值得一提的是它总是启动一个新的进程。这可能会花费多一点时间，但是它意味着当你测试你的 App 的时候，所有的状态都是全新的，这样你需要处理的可变因素就会较少了。
- **XCUIElement**.它作为你当前测试应用的 UI 元素的代理。元素都有类型和标识符以便于你找到应用中的元素。所有的元素都以网状的形式来呈现你的应用。
- **XCUIElementQuery**. 当你想要查询元素的时候，你需要使用 Element queries.每一个`XCUIElement`都支撑着一个 query。这样的查询搜索整个`XCUIElement`树，最后解析一个确切的匹配，如果未匹配到的时候，你想要访问指定元素的时候会失败，除非就是已经存在的属性，通过这个属性你可以判断一个元素是否已经在 tree 了。对于 Assertion 来说这十分有用。当你想要找到对**accessibility**可见的元素时，你可以广泛的使用`XCUIElementQuery`。`Queries`返回一系列的结果集。

现在我们已经找到了一使用 API 的方式了，是时候写一些测试了。
### Test 1 – Ensuring no navigation takes place when the switch is off

首先我们必须顶一个包含我们测试的函数。

```swift
func testTapViewDetailWhenSwitchIsOffDoesNothing() {

}
```

当函数定义完成之后，我们移动我们的鼠标到括号内，点击 Xcode 下面窗口的记录按钮。

![prerecording](http://7xl8rl.com1.z0.glb.clouddn.com/prerecording.png)

应用会立刻启动。点击 开关关闭它，然后点击详情按钮。下面的应该添加到`testTapViewDetailWhenSwitchIsOffDoesNothing`n

```swift
let app = XCUIApplication()
   app.switches["View Detail Enabled Switch"].tap()
   app.buttons["View Detail"].tap()

```

现在再次点击记录按钮，录制会立刻停止。我们可以发现应用实际上没有展示详情视图控制器，但是测试此时并不知道发生了什么。我们必须 assert提示此时没有发生任何改变。我们也可以通过检查导航栏的标题来判断。这种判断可能不适应于其它的情况，但是此时它是适用的。

```swift
XCTAssertEqual(app.navigationBars.element.identifier, "Menu")
```
当我们添加完上面一行代码时，再次运行测试，它将仍然通过测试。例如尝试改变"Menu"字符串为"Detail"，测试将会失败。下面的是一些此次测试的最终结果的一些代码，这写代码附带着注释，并且可以解释测试的行为:

```swift
func testTapViewDetailWhenSwitchIsOffDoesNothing() {
    let app = XCUIApplication()

    // Change the switch to off.
    app.switches["View Detail Enabled Switch"].tap()

    // Tap the view detail button.
    app.buttons["View Detail"].tap()

    // Verify that nothing has happened and we are still at the menu screen.
    XCTAssertEqual(app.navigationBars.element.identifier, "Menu")
}
```

### Test 2 – Ensuring navigation takes place when the switch is on


第二次测试与第一次十分相似，所以我们不会详细解释细节了。两次的唯一的区别就是Switch 开关是否启用。因此应用不该加载详情页面，并且`XCTAssertEqual `将会验证它。

```swift
func testTapViewDetailWhenSwitchIsOnNavigatesToDetailViewController() {
    let app = XCUIApplication()

    // Tap the view detail button.
    app.buttons["View Detail"].tap()

    // Verify that navigation occurred and we are at the detail screen.
    XCTAssertEqual(app.navigationBars.element.identifier, "Detail")
}
```

### Test 3 – Ensuring the increment button increments the value label.

在此次测试中，我们需要验证用户点击 increment 按钮的时候，数值标签的数字会增加1. 前两行的测试代码与之前相册，因此我们从上一个测试 copy 并且 粘贴到处。

```swift
let app = XCUIApplication()

 // Tap the view detail button to open the detail page.
 app.buttons["View Detail"].tap()

```

接着我们需要获取按钮的访问，我们想要点击它很多次，因此我们将它存储为可变的。与其手写代码去发现按钮，不得不调试它，再次启动录制，点击按钮。然后添加下面的代码：

```swift
app.buttons["Increment Value"].tap()

```

我们可以停止录制，修改成下面的:

```swift
let incrementButton = app.buttons["Increment Value"]
```

这一次我们不需要手动写代码去找到 button,对 vaule label 我们做同样的操作

```swift
let valueLabel = app.staticTexts["Number Value Label"]
```

现在我们找到了那些我们感兴趣并且准备交互的 UI 元素了。在这次测试离，我们将会验证点击按钮十次Label相应的变化。我们可以在记录器中记录十次，但是由于我们已经储存了 UI 元素了，因此我们可以简单的把它们置于一个循环内：

```swift
for index in 0...10 {
    // Tap the increment value button.
    incrementButton.tap()

    // Ensure that the value has increased by 1.
    XCTAssertEqual(valueLabel.value as! String, "\(index+1)")
}
```

上面的三个 tests离一个综合理解test体系还很远，但是这将给你一个好的学习开始，而且你也会很容易扩展学习。那么此时为什么不自己尝试添加一个测试，去验证当你开关 `switch`时， 按钮是否可用，你是否可以导航？

### When Recording Goes Wrong

有时在记录时，你点击一个元素，你会主意到 code 并没有良好运行。这常常是因为你所交互的UI元素对于`Accessibility`是不可见的。想要搞明白这种情况，你可以使用 Xcode 的`Accessibility`面板。

![accessibilityInspector](http://7xl8rl.com1.z0.glb.clouddn.com/accessibilityInspector.png)

只要它是打开的，如果你点击 **CMD+F7**,通过模拟器上的鼠标覆盖在一个 UI 元素，你会在鼠标下面找到这个元素的综合信息。这将给你一个线索为什么`Accessibility`找不到你的元素。

一旦你定位了问题，打开 interface builder，在**identity**面板上你会找到`Accessibility`面板。这将允许你开启`Accessibility`，设置提示，label 标识符和元素特征。这些是十分强大的工具去开启`Accessibility`访问你的界面。

![accessibilityPanel](http://7xl8rl.com1.z0.glb.clouddn.com/accessibilityPanel.png)

### When a Test Fails

如果测试失败，你又不确定为什么。有一些方式去帮助你修复它。首先，你可以访问**Xcode’s Report Navigator**中的测试报告。

![testFailure](http://7xl8rl.com1.z0.glb.clouddn.com/testFailure.png)

当你打开这个视图的，将鼠标移动测试的一些步骤提上下面，你会看一个小眼睛的图标在测试行为的右边。如果你点击小眼睛，你会看到一个在指定时刻的应用的状态截图。这将帮助你可视化的查看你 UI 的状态并找到哪里出错了。

就像单元测试一样，你可以在 UI 测试中添加断点。这将使得你调试行为发现问题。你可以log输出视图测层级，使用上述技术查看UI 的**accessibility** 属性去查看测试为什么会失败。

### Why you should use UI Testing

自动化 UI 测试是一个非常棒的方式，特别是给予你充足的自信去修改你的 app，提高质量保障。我们已经明白给 UI 做测试在 xcode 运行测试是多么的简单了。如果添加**Accessibility**特性到你的 app不仅帮助你测试 app，而且给予有残障用户使用你的 app 的能力的益处。

Xcode 中的 UI 测试中最棒的特性之一就是在持续集成服务器中运行你的测试的能力。同样支持的有 Xcode 的 bots,还有  [from the command line](https://krausefx.com/blog/run-xcode-7-ui-tests-from-the-command-line)，使用命令行意味着当一个测试失败时，你会立刻得到通知。

### Further Reading

关于在 Xcode 中的UI Testing的更多信息，我推荐你们去观看WWDC session 406，[UI Testing in Xcode ](https://developer.apple.com/videos/wwdc/2015/?id=406).你也能感兴趣阅读[Testing In Xcode Documentation](https://developer.apple.com/library/prerelease/ios/documentation/DeveloperTools/Conceptual/testing_with_xcode/chapters/Introduction.html#//apple_ref/doc/uid/TP40014132)和[Accessibility for Developers Documentation](https://developer.apple.com/accessibility/)
