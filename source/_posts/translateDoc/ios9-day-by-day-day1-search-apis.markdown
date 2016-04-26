---
layout: post
title: ios9-day-by-day-day1-search-apis
date: '2015-12-25 11:35'
tags:
  - 'iOS9'
  - search-apis
---

>本文翻译自[www.shinobicontrols.com](www.shinobicontrols.com)官网 ios9-day-by-day系列，原作者是Chris Grant，本文是第一课，主要讲解了新引入的 search-apis.

![ios9_cover](http://7xl8rl.com1.z0.glb.clouddn.com/ios9_cover.jpg)
<!-- more -->

在 iOS9之前，你只能根据应用的名称在 **spotlight** 搜索到相应的 apps.随着 Apple 公布出新的 iOS9 Search APIs,用户被允许从他们的 app 中选取一些内容被索引,同时包括如何出现在 spotlight 中，用户点击 spotlight中的内容会发生的行为。

### The 3 APIs

#### NSUserActivity

`NSUserActivity` 接口最早在 iOS8中因为 Handoff 的缘故被引入，但是 iO9已经允许搜索 activities 了。你现在可以为这些 activities 提供元数据了，这就意味着可以通过 soptlight 来搜索它们了。activities 扮演者历史活动记录的角色，与你使用浏览器中的浏览记录特别相似。用户现在可以通过 Spotlight 很快捷的打开他们最近的一些活动了。

#### Web Markup

Web Markup 允许 apps 镜像他们在网站上的内容被 soptlight 索引。用户没有必要因为要展示在 Spotlight 搜索结果而在他们的设备上安装你的 app。Apple 的 indexer 会根据你网站上的标记自动爬取相应的 web 搜索。在 Safari 和 Soptlight 都可以使用这个功能。

用户即设备使没有安装app也可以搜索一些东西是它的一大特色。因为它将给你的 app更多的机会出现在一些潜在用户的面前。来自你应用的深度链接作为公开的属性暴露给 Search APIs 会存储在苹果的云索引中。想要了解更多的关于 Web MarkUp,查看苹果的 [Use Web Markup to Make App Content Searchable](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html#//apple_ref/doc/uid/TP40016198-SW4)文档。

#### CoreSpotlight

**CoreSpotlight** 是 iOS9的一个新的框架,它允许你去索引你应用中的任何内容。保存用户的历史的时候，NSUserActivity就十分有用。通过这个接口，你可以索引任何你钟意的数据。它基本上保证了你最低限度的访问你设备中的 `CoreSpotlight`索引。

#### Using the Core Spotlight APIs

**NSUserActivity** 和 **Web Markup **接口很简单就可以使用，但是**CoreSpotlight**相对来说就复杂一点。为了展示下如何使用最新的 **Core Spotlight**的接口，让我们创建一个显示我们朋友列表的 app,在点击列表中的名字的时候，我们在 app 中打开一个相应的图片。 你可以在 [GitHub](https://github.com/shinobicontrols/iOS9-day-by-day/tree/master/01-Search-APIs)中找到响应的代码，然后按照下面的方式一步步构建我们的应用。

![friendApp-576x1024](http://7xl8rl.com1.z0.glb.clouddn.com/friendApp-576x1024.png)

这个 app 有个包含`FriendTableViewController`的 storyboard，FriendTableViewController中展示了我们朋友名字的列表，`FriendViewController`展示了每一个朋友的详情。

![storyboard](http://7xl8rl.com1.z0.glb.clouddn.com/storyboard.png)

关于我们朋友的所有的信息都被存储在`Datasource`类中。在`Datasource` 中，我们创建相应 的模型对象来存储我们朋友的信息，也包含了一些存储朋友到 Core Spotlight 索引的一些逻辑。

首先，在`Datasource`类中我们需要继承`init()`方法，然后创建和存储一个 Person 对象数组。 你也可能加载你自己的数据从服务器，数据库，或者其它的地方。但是对于展示的目的，我们只是简单的创建一些假的数据。

```swift
override init () {
    let becky = Person()
    becky.name = "Becky"
    becky.id = "1"
    becky.image = UIImage(named: "becky")!

    ...

    people = [becky, ben, jane, pete, ray, tom]
}
```

一旦数据存储在 `people` 数组中，`Datasource`就可以使用了。

现在数据准备就绪了，在 table view 需要 cell 展示对应书库的时侯，`FriendTableViewController`类可以创建一个`Datasource`的实例。
```swift
let datasource = Datasource()
```
在`cellForRowAtIndexPath`方法内，很简单的按照下面的方法展示表格中的内容:
```swift
let person = datasource.people[indexPath.row]
cell?.textLabel?.text = person.name
```
### Saving the person entries to Core Spotlight

现在伪造的数据已经创建完毕了，我们可以通过 iOS9提供的新接口来将数据存储在 `Core Spotlight`中。回到`Datasource`类中，我们已经定义了一个函数:`savePeopleToIndex`。当视图加载完毕后，`FriendTableViewController`就可以调用这个函数。

在这个函数内，我们通过枚举 people 数组内的每一个 person 来为每一个 person 创建一个`CSSearchableItem`对象。然后将`CSSearchableItem` 存储进一个叫`searchableItems` 的临时数组内。

```swift
let attributeSet = CSSearchableItemAttributeSet(itemContentType: "image" as String)
attributeSet.title = person.name
attributeSet.contentDescription = "This is an entry all about the interesting person called (person.name)"
attributeSet.thumbnailData = UIImagePNGRepresentation(person.image)
let item = CSSearchableItem(uniqueIdentifier: person.id, domainIdentifier:
    "com.ios9daybyday.SearchAPIs.people", attributeSet: attributeSet)
searchableItems.append(item)
```
最后一步就是调用默认`CSSearchableIndex`中的`indexSearchableItems`方法。它的主要作用就是存储 **items** 到 **CoreSpotLight** 中。此时用户可以搜索这些 person,并将结果展示在 spotlight 的搜索结果中。

```swift
CSSearchableIndex.defaultSearchableIndex().indexSearchableItems(searchableItems,
                   completionHandler: { error -> Void in
    if error != nil {
        print(error?.localizedDescription)
    }
})
```
到此，我们已经基本完成了。当你运行你的应用的时候，所有的数据都会被立刻存储。当你在 core spotlight 中搜索的时候，你想要搜索的朋友（PS:如果存在该朋友才会出现）就会出现。

### Responding to User Selection

现在用户已经可以在 SpotLight 中看到他们的搜索结果了，此时我们希望用户点击他们。但是他们点击后又会发生什么呢？好吧，此时，点击一个结果只会单纯的打开你的 app 的主页面。如果你希望展现搜索的friend的详情页面，还有一些工作需要去完成。当用户通过上述方式打开的时候，我们可以通过在 app's `Appdelegate` 中的`continueUserActivity UIApplicationDelegate`方法指定用户的具体行为。

下面是这个方法的实现：

```swift
func application(application: UIApplication, continueUserActivity userActivity: NSUserActivity, restorationHandler: ([AnyObject]?) -> Void) -> Bool {
    // Find the ID from the user info
    let friendID = userActivity.userInfo?["kCSSearchableItemActivityIdentifier"] as! String

    // Find the root table view controller and make it show the friend with this ID
    let navigationController = (window?.rootViewController as! UINavigationController)
    navigationController.popToRootViewControllerAnimated(false)
    let friendTableViewController = navigationController.viewControllers.first as! FriendTableViewController
    friendTableViewController.showFriend(friendID)

    return true
}
```

正如你所见到的，我们先前使用`indexSearchableItems`函数索引保存到`CoreSpotlight `的数据现在已经可以得到了，它们都存在`userActivity.userInfo`的字典中。我们唯一感兴趣的就是采用数据的 `friend ID`，这些 friendID 都以items的`kCSSearchableItemActivityIdentifier`形式存储起来。

一旦我们从 `userInfo`字典中解析出信息，我们可以得到应用的导航控制器，并 pop 到根视图（使用无动画 pop,这样用户就会无感知），然后我们调用`friendTableViewController`的 `showFriend`方法。我不会详细解释它是如何运作的，但基础的它找到 `datasource` 中给定 ID 的 `friend`,然后在导航视图堆栈中导航至一个新的恩视图控制器。这就就是剩下的我们需要做的。现在当用户点击 SpotLight 中的一个搜索结果时，下面将是他们会看到的：

![backToSearch-576x1024](http://7xl8rl.com1.z0.glb.clouddn.com/backToSearch-576x1024.png)

正如你所见，有一个 **“Back to Search”** 选项在视图的左上方，当他们通过点击搜索界面的 friend 的名字到当前界面的时候，他们仍然可以通过点击该选项直接返回到 spot light 的搜索界面。他们也可以通过点击标准的返回按钮进行页面的导航。

### Demo Summary

在上述 demo 中，我们已经发现使用 CoreSpotlight 的索引来集成应用的数据是多么的简单。当我们尝试让用户打开我们的APP，它是多么的强大，用户搜索指定内容时它也十分有助。

我们并没有设计到从索引中移除数据，但是保存尚未过期的应用内的索引是十分重要的。关于从 CoreSpotlight中移除一些旧的词条的信息，请查看`deleteSearchableItemsWithIdentifiers`，`deleteSearchableItemsWithDomainIdentifiers`，`deleteAllSearchableItemsWithCompletionHandler`等函数。

### The Importance of Good Citizenship
虽然在你的 Spotlight 和 Safrai 中展示尽可能多的内容看起来是个很棒的主意，但是爬取搜索 indexes 一定要三思而后行。在iOS 的生态系统中做一个安分守己的市民，不近保持用户的舒适很重要，而且苹果也会仔细审查你的 app.苹果已经很明确的投入了很多相关性的保护，参与率会被记录下来。那些通过索引作弊的人的结果会被移到搜索的最下面。

### Further Information

关于 `Search APIs`的更多信息，我推荐你们观看 WWDC 的 session 709, [Introducing Search APIs](https://developer.apple.com/videos/wwdc/2015/?id=709)
你也许会对阅读[NSUserActivity Class Reference](https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSUserActivity_Class/)和[documentation for CoreSpotlight](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html#//apple_ref/doc/uid/TP40016198-SW3)感兴趣，如果你对我们这篇文章描述和创建的项目感兴趣的话，通过 [GitHub](https://github.com/shinobicontrols/iOS9-day-by-day/tree/master/01-Search-APIs)下载它们。
