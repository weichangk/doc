

# 准备工作

- 官网 https://www.nuget.org/  注册账号
  - 创建 `API keys` 将包推送到服务器用到
- `nuget` 工具下载 https://www.nuget.org/downloads
  - 下载后可以直接放在 `System32` 文件夹下，之后便可以直接打开 `CMD`，调用 `nuget`指令查看是否能使用



# 打包项目

```shell
dotnet build --configuration Release
```



# 创建并编辑 spec 文件

生成 `.nuspec` 文件

```shell
nuget spec
```

编辑 `.nuspec `文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<package >
  <metadata>
    <id>Weick.Orm.Core</id>
    <version>1.0.0</version>
    <title>Weick.Orm.Core</title>
    <authors>weichangk</authors>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <license type="expression">MIT</license>
    <!-- <icon>icon.png</icon> -->
    <projectUrl>https://github.com/weichangk/weick.Orm.Core</projectUrl>
    <description>It is not the encapsulation of ORM，a based on EF + dapper + Autofac, is repository and unitofwork</description>
    <releaseNotes>Summary of changes made in this release of the package.</releaseNotes>
    <copyright>Copyright @2022</copyright>
    <tags>orm ef dapper Autofac unitofwork</tags>
    <dependencies>
        <dependency id="Microsoft.EntityFrameworkCore" version="3.1.3" />
        <dependency id="Microsoft.EntityFrameworkCore.InMemory" version="3.1.3" />
        <dependency id="Microsoft.EntityFrameworkCore.Proxies" version="3.1.3" />
        <dependency id="Microsoft.EntityFrameworkCore.Sqlite" version="3.1.3" />
        <dependency id="Microsoft.EntityFrameworkCore.SqlServer" version="3.1.3" />
        <dependency id="Microsoft.EntityFrameworkCore.Tools" version="3.1.3" />
        <dependency id="Pomelo.EntityFrameworkCore.MySql" version="3.1.1" />
        <dependency id="Dapper" version="2.0.30" />
        <dependency id="Autofac.Extensions.DependencyInjection" version="6.0.0" />
    </dependencies>
  </metadata>
</package>
```



# 生成 .nupkg 文件

```shell
nuget pack Weick.Orm.Core.csproj -Prop Configuration=Release
```



# 上传 nuget.org 服务器

```shell
dotnet nuget push Weick.Orm.Core.1.0.0.nupkg --api-key xxx --source https://api.nuget.org/v3/index.json
```

`--api-key xxx`为官网 https://www.nuget.org/  创建的 `API Key`



# 查看上传的包

进入 `nuget` 官网，在  `Manage Packages` 里查看上传的包

- Published Packages   已发布

- Unlisted Packages 未发布待审核

  ​                           

# 参考

`nuspec` 官网：https://docs.microsoft.com/zh-cn/nuget/what-is-nuget

`nuspec `文件描述：https://docs.microsoft.com/zh-cn/nuget/reference/nuspec

`nuget package` 文件目录：https://docs.microsoft.com/zh-cn/nuget/create-packages/creating-a-package#from-a-convention-based-working-directory