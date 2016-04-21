---
layout: post
title: 'UISearchController Tutorial: Getting Started '
date: '2016-04-21 09:45'
tags:
  - iOS8
  - UISearchController
  - iOS翻译
---

>原文发表由 Andy Pereira 发表于[raywenderlich][345cbc60]官网上苹果废弃了以前的 UISearchDisplayController， 在iOS8新引入的UISearchController，这篇文章介绍了如何入门UISearchController。

  [345cbc60]: https://www.raywenderlich.com/113772/uisearchcontroller-tutorial "raywenderlich"

<!-- more -->

注：本文环境于2016-04-03日更新到 Xcode7.3,iOS9.3,Swift2.2。
如果你的应用需要展现大量的数据，存在大量的列表时，滚动开始变得很慢，很令人沮丧。此时允许用户开始搜索特定的条目变得十分迫切。非常幸运，**UIKit** 所包换的 **UISearchBar**，使得我们很容易集成到 **UITableView**,它能够允许我们快速的过滤响应的信息。

在这篇 **UISearchController**教程中，你需要创建一个基于 table view 的可以搜索的Candy应用，你将会添加搜索的能力，包括动态过滤，一个可以选择的 Scopde bar.你将充分利用 iOS8 添加UISearchController的优势。最后你将明白如何让你的 apps 变得更加友好符合你用户迫切的需求。
<div align ='center'>
![UISearchBarController-Square](http://7xl8rl.com1.z0.glb.clouddn.com/UISearchController-Square.png?imageView/2/w/375/q/90)
</div>

### Getting Started
下载这篇教程的[初始工程](http://www.raywenderlich.com/wp-content/uploads/2015/09/CandySearch-Starter.zip)，并且打开。此应用已经设置好导航控制器和样式。编译并且运行 app，你将会看到一个空的列表如下：
<div align ='center'>
![UISearchController-Starter](http://7xl8rl.com1.z0.glb.clouddn.com/UISearchController-Starter.png?imageView/2/w/375/q/90)
</div>


回到 Xcode，文件 `Candy.swift `包含了一个类，这个类存储了你将要展示的每一个candy 的信息。这个类包含了两个属性：分类和 `candy` 的名称。

当用户在你的 app 中搜索 `candy`时，你将会根据用户搜索的字符串来引用 name属性。这篇教程的最后当你实现了 Scope Bar 的时候，你将会发现分类字符串在变得异常重要。

### Populating the Table View
打开`MasterViewController.swift`.`candies`属性将会用来管理你所搜索的所有的不同的 `candy` 对象。对了，是时候创建一些 `candy `了。

在这篇教程中，你只需要创建有限数目的的值来说明 `search bar`如何工作。在一个正式 app 中，你可能会有成千上万的可搜索对象。但是是否一个应用有成千上万的对象可以查询或者只有一小部分，方法是同样是适用的。

去计算你的 `candies`的数组，添加下面代码到 `ViewDidLoad()`中，然后调用 `super.viewDidLoad()`:
```objc
candies = [
  Candy(category:"Chocolate", name:"Chocolate Bar"),
  Candy(category:"Chocolate", name:"Chocolate Chip"),
  Candy(category:"Chocolate", name:"Dark Chocolate"),
  Candy(category:"Hard", name:"Lollipop"),
  Candy(category:"Hard", name:"Candy Cane"),
  Candy(category:"Hard", name:"Jaw Breaker"),
  Candy(category:"Other", name:"Caramel"),
  Candy(category:"Other", name:"Sour Chew"),
  Candy(category:"Other", name:"Gummi Bear")
]
```
再次直接编译并且运行你的项目。此时tableView 的代理和数据源方法都已经实现了。你将会看到一个可工作的 `table View`
<div align ='center'>
![UISearchController-Data](http://7xl8rl.com1.z0.glb.clouddn.com/UISearchController-Data.png?imageView/2/w/375/q/90)
</div>

选择table view 重的一行将会展示一个candy 响应的详细页面。
<div align ='center'>
![darkchocolate](http://7xl8rl.com1.z0.glb.clouddn.com/darkchocolate.png?imageView/2/w/375/q/90)
</div>

有太多的 candy,但是只有极短的时间去找到你想要的那个。你需要一个 `UISearchBar`

### Introducing UISearchController

如果你查看过 `UISearchController`文档，你会发现它做的东西特别少。它基本在搜索上没有起到一点点作用。这个类仅仅提供了一个标准的界面，在这个界面上用户可以找到他们想要从 app 中需要的东西。

`UISearchController` 通过与一个代理协议进行通讯，来你的 app 的其它部分知道用户正在做什么。你必须为匹配你所输入的字符串去编写实际的函数。

虽然这一开始看起来有些难，写自定义的搜索函数能够给予你对指定搜索所返回的结果充分的控制。你的用户会十分感激那些十分智能快速的搜索。

如果你曾经写过基于 table View 的搜索功能，你会十分熟悉 `UIsearchDisplayController`.从 iOS8开始，这个类被废弃了，能够简化搜索过程的UISearchController取而代之。

不幸的是，在写这篇文章的时候，Interface Builder 并不支持UISearchController，所以你必须手动创建你的 UI.在 MasterViewController.swift,添加一个新的属性:
```objc
let searchController = UISearchController(searchResultsController: nil)
```

通过不使用 `UIsearchDisplayController`初始化 `UISearchController`,你将会告知 Search Controller 你想要使用相同的视图来展现你搜索的结果。如果此时你指定一个不同的 `View Controller`,那么这个相应的这个 controller 将会用来展示搜索结果。

接着，你需要为你的 `searchController` 设置一些参数。仍然在 `MasterViewController.swift`中,添加如下到 `ViewDidLoad()`中:

```objc
searchController.searchResultsUpdater = self
searchController.dimsBackgroundDuringPresentation = false
definesPresentationContext = true
tableView.tableHeaderView = searchController.searchBar

```
下面是你添加内容的纲要:
1. `searchResultsUpdater`是 `UISearchController`的一个属性，它遵守了一个新的协议`UISearchUpdating`.这个协议让的你类可以在 UISearchBar 中输入的文字发生改变的时候被通知到。待会你需要让想要收到通知的类遵守该协议。
2. 默认的，UISearchController 将会在它出现的时候变暗背景视图。这在你为 searchResultsController使用其它的 View Controller 时变得十分有用。你已经设置当前的视图来展现结果，因此你并不想要变暗你的当前视图。
3. 通过为你的 view Controller设置`definesPresentationContext` 为 true，在 UISearchController 激活状态下，确保你在导航到其他页面上，`Search Bar` 不会保留在屏幕上。
4. 最后，你要添加 `SearchBar` 为你的 `table view`的 `tableHeaderView`。记得 `Interface Builder`现在还不兼容 `UISearchController`，上面的步骤是必须的。

### UISearchResultsUpdating and Filtering

在设置搜索控制器之后，你需要进行一些编码工作让它起效果。首先在 接近`MasterViewContoller` 文件的上面添加如下的属性
```
var filterCandies = [Candy]()
```

这个属性将会持有用户搜索的 candies数组。其次，添加如下的 帮助方法到主 `MasterViewController` 类中：

```objc
func filterContentForSearchText(searchText: String, scope: String = "All") {
  filteredCandies = candies.filter { candy in
    return candy.name.lowercaseString.containsString(searchText.lowercaseString)
  }

  tableView.reloadData()
}
```
这基于搜索文字过滤了 `candies`数组，然后把搜索的结果放到你刚添加的`filteredCandies`数组中。现在不要担心 `Scope` 参数 你会在接着的下一章节中使用它。
为了使得`MasterViewController`响应 search Bar,它必须要实现`UISearchResultsUpdating`协议。打开`MasterViewController.swift `文件和添加下面的类扩展，在主 `MasterViewController.swift `之外:
```objc
extension MasterViewController: UISearchResultsUpdating {
  func updateSearchResultsForSearchController(searchController: UISearchController) {
    filterContentForSearchText(searchController.searchBar.text!)
  }
}
```
`updateSearchResultsForSearchController(_:) `方法是`UISearchResultsUpdating `协议中唯一你必须要实现的方法。
现在，无论用户在 searchBar中添加或者删除文本，UISearchController 将会通过这个方法通知 MasterViewController 类文字的改变。这个方法仅仅使用当前 searchBar 中的文本调用了一个帮助方法(待会你会定义这个方法)。

`filter()`接受一个闭包类型`(candy: Candy)  ->Bool`。它将会循环所有的元素数组，并且为数组重的没个元素调用闭包，传递当前的元素。

你可以使用这来决定是否一个 candy 是搜索结果的一部分来呈现给用户。为此，你需要根据当前 candy 是否包含在 filterArray 中来传递 true,不在传递 false.

为了决定这，containsString(_:)常被用来检查candy 的 name是否包含 searchText.但是在做比较前，你需要使用l`owercaseString`方法转化搜索的和匹配的文字为等同的小写文字。

>注： 大多时候，当执行一个搜索的时候，通过只比较他们输入的candy 的名字小写版本，可以很容易返回一个大小写敏感的匹配，用户根本不会为大小写所烦恼。现在你输入一个 “Chocolate” 或者 “chocolate” ，任一个都会返回一个匹配的 candy。非常棒的一个技巧

现在编译并且运行你的 app，你将会注意到并没有一个 searchBar 在 表格上。
然后，你输入任何搜索问哦，你任然看不到过滤的结果。发生什么了？只是因为你还没有开始写代码让表格知道什么时候使用过滤的结果。

回到`MasterViewController.swift`中，将`tableView(_:numberOfRowsInSection:)`替换成下面的:

```objc
override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  if searchController.active && searchController.searchBar.text != "" {
    return filteredCandies.count
  }
  return candies.count
}
```

还没有发生任何改变，你只是简单的检查了用户有没有在搜索，并且使用过滤的还是正常的 candies 作为 表格的数据源。

然后，替换`tableView(_:cellForRowAtIndexPath:) `为：

```objc
override func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
  let cell = tableView.dequeueReusableCellWithIdentifier("Cell", forIndexPath: indexPath)
  let candy: Candy
  if searchController.active && searchController.searchBar.text != "" {
    candy = filteredCandies[indexPath.row]
  } else {
    candy = candies[indexPath.row]
  }
  cell.textLabel?.text = candy.name
  cell.detailTextLabel?.text = candy.category
  return cell
}
```

两个方法都涉及到了使用 `searchController`的actives属性来决定展示哪一个数组。

当用户点击 searchBar的搜索框的时候，active属性会自动设置为 `ture`。如果 搜索控制器被激活，然后你将会看到用户实际在搜索框输入了什么东西。如果他们确实输入了东西，数据来自于`filteredCandies`数组，否则的话来自所有的条目中。

回忆下搜索控制器自动处理展示和隐藏搜索结果 table，因此你的代码所需要做的就是提供正确的数据(过滤的和非过滤的)基于控制器的状态和用户是否已经搜索了一些东西。
<div align ='center'>
![ragecomic](http://7xl8rl.com1.z0.glb.clouddn.com/ragecomic.png?imageView/2/w/375/q/90)
</div>
编译并且运行你的 app.你会得到一个带有搜索功能的 search Bar来过滤主表格的数据。好棒！

<div align ='center'>
![UISearchController-Filter](http://7xl8rl.com1.z0.glb.clouddn.com/UISearchController-Filter.png?imageView/2/w/375/q/90)
</div>

玩一会你的 app去看看搜索不同类型candy的结果。
仍然还有一个问题，当你从搜索列表重去选择一行的时候，你可能注意到详情页显示的 candy 是错误的。是时候修复它了。

### Sending Data to a Detail View
当我们发送信息到详情视图控制器中的时候，你需要确保视图控制器知道用户正在工作的上下文：整个表单？还是搜索的结果。仍然在`MasterViewController.swift,`文件中，在`prepareForSegue(_:sender:)`方法中，找到下面的代码:
```objc
let candy = candies[indexPath.row]
```
然后用如下代码替换它
```objc
let candy: Candy
if searchController.active && searchController.searchBar.text != "" {
  candy = filteredCandies[indexPath.row]
} else {
  candy = candies[indexPath.row]
}
```
此时你需要在 `tableView(_:numberOfRowsInSection:)` 和 `tableView(_:cellForRowAtIndexPath:) `相似的检查。但是当执行一个到详情控制器的时候，你需要提供合适的 candy 对象。

编译运行代码，将会看到无论是在主table中，还是在搜索的时候，你的 app都会导航到正确的详情视图。

<div align ='center'>
![CandySearch-DetailView](http://7xl8rl.com1.z0.glb.clouddn.com/CandySearch-DetailView.png?imageView/2/w/375/q/90)
</div>

### Creating a Scope Bar to Filter Results

如果你想给你的用户另一种方式去过滤他们的结果，你可以添加一个Scope。结合你的 SearchBar,并通过它们的分类过滤所有的条目。你想过滤的分类就是那些你赋值给 在创建创建你的 candies 数组时： Chocolate,Hard和 Other，时赋值给你的Candy 对象。
首先，你必须创建一个 Scope bar 在`MasterViewController`.scope bar 是一个带有分隔控件，在指定的 scopes 中进行搜索可以缩小你的搜索范围。一个 Scope 完全按照你的定义来决定它的意义。在这种情况下，它就是 candy 的分类。但是scopes 可以是类型，范围，或者完全其它的东西。

实现一个额外的代理协议就可以很简单的使用一个 Scope Bar。

在MasterViewController.swift文件中，你需要添加另外一个扩展去遵守`UISearchBarDelegate`协议，添加下面的在`UISearchResultsUpdating`协议扩展之后:

```objc
extension MasterViewController: UISearchBarDelegate {
  func searchBar(searchBar: UISearchBar, selectedScopeButtonIndexDidChange selectedScope: Int) {
    filterContentForSearchText(searchBar.text!, scope: searchBar.scopeButtonTitles![selectedScope])
  }
}
```
这个代理方法会在用户切换 scope bar 中的 scope 的时候调用。当方法调用时，你想要重新过滤，因此你需要调用`filterContentForSearchText(_:scope:) `在选择新的 scope 时。

现在修改`filterContentForSearchText(_:scope:) `去考虑新提供的 scope:

```objc

func filterContentForSearchText(searchText: String, scope: String = "All") {
  filteredCandies = candies.filter { candy in
    let categoryMatch = (scope == "All") || (candy.category == scope)
    return  categoryMatch && candy.name.lowercaseString.containsString(searchText.lowercaseString)
  }

  tableView.reloadData()
}
```
这会检查是否所提供的 scope 要么被设置成全部，或者匹配到相应的 candy 分类。只有满足情况是，被测试的 candy 才会被添加到`filteredCandies`中。

你基本上完成了，但是 scope过滤机制还没有良好的工作。你需要修改updateSearchResultsForSearchController(_:)  在第一个你创造去发送当前选择 scope 的类扩展。

```objc
func updateSearchResultsForSearchController(searchController: UISearchController) {
  let searchBar = searchController.searchBar
  let scope = searchBar.scopeButtonTitles![searchBar.selectedScopeButtonIndex]
  filterContentForSearchText(searchController.searchBar.text!, scope: scope)
}
```
唯一遗留的问题就是用户到现在还没看到一个 scope Bar.为了添加一个 Scope bar ，找到`MasterViewController.swift,`，在`viewDidLoad()`中设置 searchController的后面添加如下的代码:

```objc
searchController.searchBar.scopeButtonTitles = ["All", "Chocolate", "Hard", "Other"]
searchController.searchBar.delegate = self
```
这将添加一个 带有匹配你赋值给 candy对象的分类的scope Bar到 seach bar 中。你将会包含一个匹配所有的分类叫做 All的 scope，它将用来忽略candy 分类这个属性。

现在，当你输入的时候，被选择的 scope button 将会与搜索文字结合起来被使用。

编译并且运行你的 app。随意输入一些搜索文字和改变 scope。
<div align ='center'>
![UISearchController-Filter](http://7xl8rl.com1.z0.glb.clouddn.com/UISearchController-Filter.png?imageView/2/w/375/q/90)
</div>

输入 "caramel"选择 scope 到全部。它将会显示在列表中，当你将 scope 改为巧克力的时候，"caramel"会消失，因为它并不是一种巧克力。

### Where To Go From Here?

恭喜你。现在你有一个可以工作的 app,它允许你直接在 主列表重进行搜索。这里是一个[可下载的完整的 demo][8613dfb1]

  [8613dfb1]: http://www.raywenderlich.com/wp-content/uploads/2015/09/CandySearch.zip "downloadable sample project"
tableView 被各种类型的 app 所使用,提供一个搜索选项是一个非常适用的能力。通过  `UISearchBar`和 `UISearchController`,iOS提供了在盒子外的绝大部分功能，因此我们没有理由不使用它。搜索一个特别大的 tableview 是用户特别期待的功能，当没有这个功能的时候。他们可能能就不再开心的使用你的 app了。

![nosearchfunction](http://7xl8rl.com1.z0.glb.clouddn.com/nosearchfunction.jpg)

我希望以后可以看到你在你的tableView中添加了搜索能力。如果你有任何问题和想要评论，加入下面的论坛讨论。
