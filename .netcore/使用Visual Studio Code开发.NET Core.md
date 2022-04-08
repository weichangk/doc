安装 .Net Core 扩展，比如c#



一个解决方案



添加类库并将其添加到解决方案中



添加测试项目并运行测试



添加一个控制台程序并使用我们的库





# 安装  .Net Core

https://dotnet.microsoft.com/en-us/download



安装后访问命令行工具，输入 dotnet --help，可以看到基本命令

```shell
PS C:\Users\weichangk> dotnet --help
.NET SDK (6.0.100)
使用情况: dotnet [runtime-options] [path-to-application] [arguments]

执行 .NET 应用程序。

runtime-options:
  --additionalprobingpath <path>   要探测的包含探测策略和程序集的路径。
  --additional-deps <path>         指向其他 deps.json 文件的路径。
  --depsfile                       指向 <application>.deps.json 文件的路径。
  --fx-version <version>           要用于运行应用程序的安装版共享框架的版本。
  --roll-forward <setting>         前滚至框架版本(LatestPatch, Minor, LatestMinor, Major, LatestMajor, Disable)。
  --runtimeconfig                  指向 <application>.runtimeconfig.json 文件的路径。

path-to-application:
  要执行的应用程序 .dll 文件的路径。

使用情况: dotnet [sdk-options] [command] [command-options] [arguments]

执行 .NET SDK 命令。

sdk-options:
  -d|--diagnostics  启用诊断输出。
  -h|--help         显示命令行帮助。
  --info            显示 .NET 信息。
  --list-runtimes   显示安装的运行时。
  --list-sdks       显示安装的 SDK。
  --version         显示使用中的 .NET SDK 版本。

SDK 命令:
  add               将包或引用添加到 .NET 项目。
  build             生成 .NET 项目。
  build-server      与由生成版本启动的服务器进行交互。
  clean             清理 .NET 项目的生成输出。
  format            将样式首选项应用到项目或解决方案。
  help              显示命令行帮助。
  list              列出 .NET 项目的项目引用。
  msbuild           运行 Microsoft 生成引擎(MSBuild)命令。
  new               创建新的 .NET 项目或文件。
  nuget             提供其他 NuGet 命令。
  pack              创建 NuGet 包。
  publish           发布 .NET 项目进行部署。
  remove            从 .NET 项目中删除包或引用。
  restore           还原 .NET 项目中指定的依赖项。
  run               生成并运行 .NET 项目输出。
  workload          管理可选工作负荷。

捆绑工具中的其他命令:
  dev-certs         创建和管理开发证书。
  fsi               启动 F# 交互/执行 F# 脚本。
  sql-cache         SQL Server 缓存命令行工具。
  user-secrets      管理开发用户密码。
  watch             启动文件观察程序，它会在文件发生更改时运行命令。

运行 "dotnet [command] --help"，获取有关命令的详细信息。
```

参考：https://docs.microsoft.com/zh-cn/dotnet/core/tools/?tabs=netcore2x



# 安装  vs code 

https://code.visualstudio.com/download



# 配置 .Net Core 开发相应扩展



# 创建一个解决方案

- 为解决方案创建目录
- 调用生成解决方案的命令

```shell
PS D:\baocai\.net> mkdir HelloBlog


    目录: D:\baocai\.net


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          2022/3/2     21:47                HelloBlog


PS D:\baocai\.net> cd helloblog
PS D:\baocai\.net\helloblog> dotnet new sln
已成功创建模板“解决方案文件”。
PS D:\baocai\.net\helloblog> 
```



# 创建一个类库

```shell
PS D:\baocai\.net\HelloBlog\src> dotnet new classlib -o Blog.Framwork
已成功创建模板“类库”。

正在处理创建后操作...
在 D:\baocai\.net\HelloBlog\src\Blog.Framwork\Blog.Framwork.csproj 上运行 “dotnet restore”...
  正在确定要还原的项目…
  已还原 D:\baocai\.net\HelloBlog\src\Blog.Framwork\Blog.Framwork.csproj (用时 60 ms)。
已成功还原。

PS D:\baocai\.net\HelloBlog\src> 
```

指定项目版本

```shell
dotnet new console -o Blog.Framwork --framework net6.0
```



项目类型指令

- classlib 标准类库

- web 普通web

- mvc mvc项目

- webapi webapi

- console 控制台

- -o xxx 表示在当前文件夹所在的位置输出一个文件夹

- --framework net6.0 指定版本


# 向解决方案添加类库项目

```shell
PS D:\baocai\.net\HelloBlog> dotnet sln add .\src\Blog.Framework\Blog.Framework.csproj 
已将项目“src\Blog.Framework\Blog.Framework.csproj”添加到解决方案中。
```



# 从NuGet存储库中下载一个NuGet包

NuGet官网 https://www.nuget.org/

```shell
PS D:\baocai\.net\HelloBlog\src\Blog.Framework\log> cd ..
PS D:\baocai\.net\HelloBlog\src\Blog.Framework> dotnet add package log4net --version 2.0.12
  正在确定要还原的项目…
  Writing C:\Users\weichangk\AppData\Local\Temp\tmp934A.tmp
info : 正在将包“log4net”的 PackageReference 添加到项目“D:\baocai\.net\HelloBlog\src\Blog.Framework\Blog.Framework.csproj”。
info : 正在还原 D:\baocai\.net\HelloBlog\src\Blog.Framework\Blog.Framework.csproj 的包...
info : 包“log4net”与项目“D:\baocai\.net\HelloBlog\src\Blog.Framework\Blog.Framework.csproj”中指定的所有框架均兼容。
info : 包“log4net”(版本为 2.0.12)的 PackageReference 已添加到文件“D:\baocai\.net\HelloBlog\src\Blog.Framework\Blog.Framework.csproj”。
info : 正在提交还原...
info : 将资产文件写入磁盘。路径: D:\baocai\.net\HelloBlog\src\Blog.Framework\obj\project.assets.json
log  : 已还原 D:\baocai\.net\HelloBlog\src\Blog.Framework\Blog.Framework.csproj (用时 142 ms)。
PS D:\baocai\.net\HelloBlog\src\Blog.Framework> 
```



# 添加项目引用

```shell
PS D:\baocai\.net\DenpendencyInjectionDemo> dotnet add .\Demo.Application\Demo.Application.csproj reference .\Demo.Domain\Demo.Domain.csproj
已将引用“..\Demo.Domain\Demo.Domain.csproj”添加到项目。
PS D:\baocai\.net\DenpendencyInjectionDemo> 
```





参考：https://docs.microsoft.com/zh-cn/dotnet/core/tutorials/library-with-visual-studio-code?pivots=dotnet-6-0



