## 一、安装各种强大的VS Code插件
#### C# extension for Visual Studio Code

这个插件最重要的功能：

Lightweight development tools for .NET Core.

Great C# editing support, including Syntax Highlighting, IntelliSense, Go to Definition, Find All References, etc.

Debugging support for .NET Core (CoreCLR). NOTE: Mono debugging is not supported. Desktop CLR debugging has limited support.

Support for project.json and csproj projects on Windows, macOS and Linux.

#### C# Extensions
这个插件最有用的功能是可以右键新建C#类和C#接口，同时支持各种code snippets，例如 ctor 、prop等，具体功能特性，可以查看插件的说明。

####  Auto-Using for C#
这个插件自动添加using引用。

#### vscode-solution-explorer
这个插件给VS Code增加了解决方案tab, 支持新建解决方案、新建工程、添加引用、Nuget包，这个插件非常有用

Adds a Solution Explorer panel where you can find a Visual Studio Solution File Explorer.

#### Visual Studio Keymap
该插件可以将常用的 Visual Studio 快捷键映射到 VSCode 中，比如格式化代码快捷键 Ctrl+K+D

#### Code Runner
即选中一段代码，直接run

#### vscode-icons
通过这个插件，给各个文件和文件夹一个你更熟悉的图标

#### Visual Studio IntelliCode
VS代码智能提示，根据上下文语境，自动推荐你下一步用到的代码，后台基于AI的

#### NuGet Package Manager
#### NuGet NuPkg Viewer
Nuget包管理，快速查询定位Nuget包，并安装。不过尝试了一下午自定义Nuget源，没搞定，估计是URL不对

目前添加nuget包，由于国内你懂的原因，导致在查询版本的时候会报错，无法正常安装，建议大家直接右键 csproj 项目文件添加相应的nuget包，关于这个问题，大家可以关注github上的 Issue

这篇博文解决貌似可以解决这个问题
https://www.cnblogs.com/lori/archive/2019/10/10/11651079.html 



#### C# XML Documentation Comments
该插件主要是可以方便的添加代码注释，例如在Visual Studio 中的 ///



#### Razor Snippets

#### Razor+

这两个主要用于.cshtml视图文件



#### Docker
#### Kubernetes
#### Beautify
#### Chinese (Simplified)
#### GitLens — Git supercharged
#### open in browser
#### Settings Sync
#### Terminal
#### Swagger Viewer
#### YAML
#### .NET Core Test Explorer



## 二、创建.NET Core解决方案和工程
此时，VS Code的环境基本配置差不多了，接下来有两种模式，创建解决方案和工程。

#### 1. 通过vscode-solution-explorer
解决方案有了，很熟悉的感觉。

我们可以继续创建工程：右键sln，Add new project：

此时会弹出工程模板，此时我们选择ASP.NET Core Web API工程

选择语言C#

然后继续输入工程名称：例如 TestWebApi

#### 2. 通过Dotnet CLI命令行
新建sln：
```
dotnet "new" "sln" "-n" "EricTest" "-o" "e:\Work\ServiceDependency"
```
新建ASP.NET Core WebAPI工程
```
dotnet "new" "webapi" "-lang" "C#" "-n" "TestWebApi" "-o" "TestWebApi"
```
将TestWebApi工程添加到解决方案EricTest
```
dotnet "sln" "e:\Work\ServiceDependency\EricTest.sln" "add" "e:\Work\ServiceDependency\TestWebApi\TestWebApi.csproj"
```



## 三、调试运行

在Debug选项卡中新增添加调试配置（launch.json）

保存后，选择项目启动调试





--------------
https://www.cnblogs.com/tianqing/p/11874558.html