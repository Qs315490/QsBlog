---
title: 一种绕过 Windows WebView DevTools 限制的方法
tags: []
categories: []
description: ''
date: 2026-07-27 14:14:02
updated: 2026-07-27 14:14:02
---
# 背景
从 Tauri 打包的程序中使用`tauri-dumper`提取出静态资源后，对一些 Tauri 程序进行了逆向。但是每次提取、修改、重打包非常的繁琐，我还是觉得 DevTools 比较好用。但是这类 Webview 程序一般都会禁用 DevTools，如何开启成了一个问题。

# 尝试
根据[微软的文档](https://learn.microsoft.com/zh-cn/microsoft-edge/webview2/how-to/debug-devtools?tabs=dotnetcsharp)
> 如果上述方法均不可用，可以通过环境变量或注册表项添加到 `--auto-open-devtools-for-tabs` 浏览器参数。 创建 WebView2 时，此方法将打开 DevTools 窗口。

我测试了 Chrome 系的浏览器，发现在启动时加上`--auto-open-devtools-for-tabs`参数后，确实会自动打开 DevTools。但是 Webview 没有启动参数，如何传递参数给 Webview 呢？ 注意到微软文档提到了可以添加到环境变量和注册表，虽然没有详细说明，但是经过一番查找还是让我找到了[文档](https://learn.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2environment.createasync?view=webview2-dotnet-1.0.1774.30)。

# 解决
文档中提到
> When creating a CoreWebView2Environment the following environment variables are verified.
>
> WEBVIEW2_BROWSER_EXECUTABLE_FOLDER
> WEBVIEW2_USER_DATA_FOLDER
> WEBVIEW2_ADDITIONAL_BROWSER_ARGUMENTS
> WEBVIEW2_RELEASE_CHANNEL_PREFERENCE

不难看出，其中`WEBVIEW2_ADDITIONAL_BROWSER_ARGUMENTS`就是我们需要的环境变量，我们只需要将`--auto-open-devtools-for-tabs`添加到环境变量中，就可以在程序启动时自动打开 DevTools 了。

注意，由于 Chrome 的一些限制，我们需要在程序启动前设置环境变量，即不能运行着一个程序实例时设置环境变量再次启动，否则可能会导致程序崩溃。

# 省流
CMD
```cmd
set WEBVIEW2_ADDITIONAL_BROWSER_ARGUMENTS="--auto-open-devtools-for-tabs"
tauri-app.exe
```
PowerShell
```powershell
$env:WEBVIEW2_ADDITIONAL_BROWSER_ARGUMENTS="--auto-open-devtools-for-tabs"
tauri-app.exe
```

经过测试，使用 Webview2 来实现的程序都可以使用这种方法打开 DevTools，包括 Tauri、Wails、pywebview 等。