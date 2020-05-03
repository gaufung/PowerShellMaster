# 成为 PowerShell 大师

## 1 前言
对于从 `*nix` 过来的小伙伴，对于 `bash` 操作肯定不陌生，比如 awk, sed 和 grep 等命令用的飞起。来到 Windows 平台，cmd.exe 简直难用到爆炸，那么为什么不尝试使用 PowerShell 这个工具呢？

本教程将涉及以下几个方面：

1. PowerShell 的配置和安装
2. PowerSehll 使用说明
3. PowerShell 基本语法
4. PowerShell 的最佳实践

### 1.1 简介

首先什么是 PowerShell 呢？它是一套自动化任务和配置管理框架，主要由命令行和脚本语言构成。原先它是 Windows 的一部分，在 2016 年的时候微软将它[开源](https://github.com/PowerShell/PowerShell)并且跨平台，原先的 PowerShell 是基于 .Net Framework 现在基于 .Net Core, 所以跨平台的版本也叫做 PowerShell Core。

PowerShell 可以执行下面四种命令：
1. Cmdlet: 也就是常见的 `动词-名词` 组成的命令，比如 Get-Process, Remove-Item 等等。
2. PowerShell 脚本：以 ps1 后缀名的文件，通常是一系列命令组成的文件。
3. PowerShell 函数：在上下文中定义的函数，可以通过 PowerShell 调用。
4. 单独的可执行程序：对于可执行的程序，直接启动。

在技术论坛上，关于 PowerShell 和 Bash 的比较层出不穷，这里不去争论两者的优劣，每个工具都有自己的擅长和不足，正如 [Anders Hejlsberg](https://en.wikipedia.org/wiki/Anders_Hejlsberg) 说过:
> If you show me a perfect language, I can prove that nobody use it. 

我个人的观点是，如果是一些简单的任务，bash 是更加得心应手，因为这个符合 *inx 的设计哲学：Less is more。但是如果涉及到复杂而又繁琐的任务，PowerShell 更加得心应手一点，能够将控制项目的规模。 

### 1.2 安装

如果你是 Window 用户，那么系统中已经自带了 PowerShell, 只需要在开始搜索框内输入 PowerShell，点击即可启动 PowerShell；当然你也可以在 Windows 应用商城或者 [Github](https://github.com/microsoft/terminal) 购买或者下载 Terminal 软件，这是更加现代化的终端。对于 Linux/MacOS 用户，可以 [Github Powershell](https://github.com/PowerShell/PowerShell) 页面下载对应的版本。

当然可以查阅官方文档：
- [Windows 安装](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-windows?view=powershell-7)
- [Linux 安装](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-linux?view=powershell-7)
- [MacOS 安装](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-7)

在安装完毕，打开 PowerShell 交互界面，输入 `$PSVersionTable` 

```
PS C:\Program Files\PowerShell\7> $PSVersionTable

Name                           Value
----                           -----
PSVersion                      7.0.0
PSEdition                      Core
GitCommitId                    7.0.0
OS                             Microsoft Windows 10.0.18363
Platform                       Win32NT
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0…}
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
WSManStackVersion              3.0
```

从输出内容可以知道，当前的使用的 PowerShell Core 7.0 版本，而且运行在 Windows 平台上。

大部分人吐槽 Powershell 的界面特别丑，因为它的默认是这样的：

![](images/defaultApperance.png)

这也难怪 Steve Jobs 曾经说过，Microsoft 最大的问题是没有品味；但是毛主席也教导我们：自己动手，丰衣足食。*nix 下 bash 好看是因为大量的第三方主题配置，所以我们现在为 PowerShell 修改它的默认配置。

//todo

最后 PowerShell 页面样式是这样的
![](images/final.png)

### 1.3 编码和测试

#### 1.3.1 Get-Help 

在 Bash 中有一个很棒的 man 命令，它可以查询任意命令的详细文档。在 PowerShell 使用 Get-Help 命令也可以达到同样的效果，比如说：

```ps
➜  ~ Get-Help Get-ChildItem

NAME
    Get-ChildItem

SYNOPSIS
    Gets the files and folders in a file system drive.


SYNTAX
    Get-ChildItem [[-Filter] <String>] [-Attributes {ReadOnly | Hidden | System | Directory | Archive | Device | Normal | Temporary | SparseFile | ReparsePoint | Compressed | Offline | NotContentIndexed | Encrypted | IntegrityStream |
    NoScrubData}] [-Depth <UInt32>] [-Directory] [-Exclude <String[]>] [-File] [-Force] [-Hidden] [-Include <String[]>] -LiteralPath <String[]> [-Name] [-ReadOnly] [-Recurse] [-System] [-UseTransaction] [<CommonParameters>]

    Get-ChildItem [[-Path] <String[]>] [[-Filter] <String>] [-Attributes {ReadOnly | Hidden | System | Directory | Archive | Device | Normal | Temporary | SparseFile | ReparsePoint | Compressed | Offline | NotContentIndexed |
    Encrypted | IntegrityStream | NoScrubData}] [-Depth <UInt32>] [-Directory] [-Exclude <String[]>] [-File] [-Force] [-Hidden] [-Include <String[]>] [-Name] [-ReadOnly] [-Recurse] [-System] [-UseTransaction] [<CommonParameters>]

    Get-ChildItem [-Attributes <FileAttributes]>] [-Directory] [-File] [-Force] [-Hidden] [-ReadOnly] [-System] [-UseTransaction] [<CommonParameters>]


DESCRIPTION
    The Get-ChildItem cmdlet gets the items in one or more specified locations. If the item is a container, it gets the items inside the container, known as child items. You can use the Recurse parameter to get items in all child
    containers.

    A location can be a file system location, such as a directory, or a location exposed by a different Windows PowerShell provider, such as a registry hive or a certificate store.
    In a file system drive, the Get-ChildItem cmdlet gets the directories, subdirectories, and files. In a file system directory, it gets subdirectories and files.

    By default, Get-ChildItem gets non-hidden items, but you can use the Directory, File, Hidden, ReadOnly, and System parameters to get only items with these attributes. To create a complex attribute search, use the Attributes
    parameter. If you use these parameters, Get-ChildItem gets only the items that meet all search conditions, as though the parameters were connected by an AND operator.

    Note: This custom cmdlet help file explains how the Get-ChildItem cmdlet works in a file system drive. For information about the Get-ChildItem cmdlet in all drives, type "Get-Help Get-ChildItem -Path $null" or see Get-ChildItem at
    http://go.microsoft.com/fwlink/?LinkID=113308.


RELATED LINKS
    Online version: http://technet.microsoft.com/library/hh847897(v=wps.630).aspx
    Get-ChildItem (generic); http://go.microsoft.com/fwlink/?LinkID=113308
    FileSystem Provider
    Clear-Content
    Get-Content
    Get-ChildItem
    Get-Content
    Get-Item
    Remove-Item
    Set-Content
    Test-Path

REMARKS
    To see the examples, type: "get-help Get-ChildItem -examples".
    For more information, type: "get-help Get-ChildItem -detailed".
    For technical information, type: "get-help Get-ChildItem -full".
    For online help, type: "get-help Get-ChildItem -online"
```

如果你的 `Get-Help` 没有输出相应的内容，使用 `Update-Help` 命令更新一下。

#### 1.3.2 PowerShell Gallery

正如 NuGet 包一样，PowerShell 也也有一个包仓库: [Powershell Gallery](https://www.powershellgallery.com/)，比如说安装 `NetworkingDsc` 这个包

```ps
Install-Module -Name PSSoftware
```

通常这个命令是需要管理员权限执行，在安装完毕后，就可以使用 `PSSoftware` 包提供的[命令](https://github.com/adbertram/PSSoftware)。

#### 1.3.3 IDE

所谓是工欲善其事，必先利其器。虽说脚本语言胜在灵活多变，我们也知道 PowerShell 对于大型工程而言是具有优势的，好的 IDE 也有助于写出健壮的代码。

- Wihdows PowerShell ISE. 这是 Windows 平台自带的一个 IDE
- Visual Studio Code. 这是一个跨平台的代码编辑器，安装相关的插件可以搭建 PowerShell 开发环境
- Jupyter Notebook. 如果你是 Python 的开发这，对这个开发工具肯定是非常熟悉，现在它也有了 PowerShell 的 [kernel](https://devblogs.microsoft.com/powershell/public-preview-of-powershell-support-in-jupyter-notebooks/)。

## 2 语法

## 3 最佳实践

