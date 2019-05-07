# MANGA

Smaller, faster and less dependent [manga](https://github.com/Hentioe/manga)

## Copyright

The purpose of this project has never been to disseminate pirated resources, and the resources obtained through this tool are prohibited for profit.

The following instructions need to be added to the derivative project, e.g:

> ### 版权信息
>
> 图书名：[图书名]  
> 来源于：[来源站点或平台链接]  
> 操作人：manga-bot ([版本])  
> 本图书由开源项目: [MANGA-RS](https://github.com/Hentioe/manga-rs) 生成，资源来自于第三方。  
> **注：公开传播则意味着存在被版权方追究责任的风险。**

ENG VERSION:

> ### Copyright
>
> Book name：[name]  
> From：[Source link]  
> Operator：manga-bot ([version])  
> This book is generated by the open source project: [MANGA-RS](https://github.com/Hentioe/manga-rs), and the resources come from third parties.  
> **Warning: Public dissemination means there is a risk of being held accountable by the copyright owner.**

## 当前状态

此程序是对 [manga](https://github.com/Hentioe/manga) 项目的重新实现，目前正接近于第一个正式版发布，需要意见和测试（请轰炸 Issues）。

### 使用演示

![演示动画](https://raw.githubusercontent.com/Hentioe/manga-rs/master/demo/usage.gif)

### 目的

本项目的初衷是为了方便我个人将在线的漫画资源导入阅读器设备，不过类似 youtube-dl 不仅仅只是下载 YouTube 视频，我想支持的平台也没有上限。  
使用 Rust 重新实现是为了减少体积、依赖项并兼容到更多的平台上，如手机（Android）甚至是路由器（OpenWrt）。

### 说明

1.0 TODOs 已接近完成，最基本的功能和用法已经确定。因为此项目是纯命令行工具（GUI 版在开发中），完整的功能可以通过 `--help` 参数查看。下面举例几个常见的用法：

- 基本功能

  ```bash
  manga https://manhua.dmzj.com/yiquanchaoren/
  ```

  上面的命令会输出动漫之家的《一拳超人》漫画每一话的完整列表，可以用单个数字(1)或多个数字(1,2,3)或范围(1-3)或剔除某一项(^2)又或者组合起来(1,2,3-17,^9)使用以达到多选的目的。选择完成以后将会排队处理每一个具体的漫画链接。

- 直接保存

  ```bash
  manga https://manhua.dmzj.com/yiquanchaoren/19842.shtml#@page=1
  ```

  上面的链接是《一拳超人特别篇》的阅读链接，使用这种具体阅读链接（在 manga 中被称作 `Section`）作为参数会直接进行下载和导出操作。

- 指定导出格式

  ```bash
  manga -f epub,pdf https://manhua.dmzj.com/yiquanchaoren/19842.shtml#@page=1
  ```

  上面使用 -f 指定了两种格式(Format)，用逗号分隔，不能用空格。-f 会覆盖默认的 zip 导出格式。另外还支持 Kindle 设备常见的 mobi 和 azw3。

- 指定导出目录

  ```bash
  manga -o . https://manhua.dmzj.com/yiquanchaoren/19842.shtml#@page=1
  ```

  上面使用 -o 指定了导出文件的输出目录(Output)，`.` 表示当前文件夹，可以指定存在或不存在（会自动创建）的目录名，默认路径为 `manga_res/outputs`。

- 纯交互模式

  直接执行 manga 会进入一个纯交互模式，由你自己选择平台，翻看漫画列表并选中漫画以进行最后的保存，注意：纯交互模式在启动时也可以加上除 url 以外的其它参数（例如 -f, -o 等）。因为 Windows 上的终端软件的体验过差，一般情况下建议 Windows 用户直接双击启动 manga，默认的终端支持进度条等正常显示（cmd 和 powershell 会显得很凌乱），但是直接双击启动会不太容易传递其它参数（可以自己写 .bat 启动或等待我以后可能添加的更多针对 Windows 的交互，如果不在意凌乱的进度条输出当然也可以通过 cmd 或 powershell 启动）。

- 清理缓存

  ```bash
  manga clean
  ```

  这里的缓存指的并不是导出漫画时下载的原始图片，相反的是缓存明确的不包括原始图片，但是会利用原始图片避免重复下载（这个有点绕，不重要）。在 `manga_res` 的子目录中会有一个 `.cache` 目录，里边会存放构建 epub 文件时的完整结构目录以及 epub 文件，有时候它们会造成 `manga-bot` 升级以后仍然输出的是原来的版本的问题，此时应该清理掉缓存。清理缓存永远不会造成重复的下载。

## 安装使用

请前往 [releases](https://github.com/Hentioe/manga-rs/releases) 页面下载释出的最新版本。由于某些嵌入式系统的编译（例如 OpenWrt）相对复杂，而我个人的路由器是 ipq40xx 平台的，所以暂且只上传此平台的 ipk 包，但有需要的可以提 Issue 我会在以后使用服务器 CI 的时候加入支持。

## FAQ

- `manga-bot` 和 `manga` 有什么区别？为什么版本不一样？

  严格来说 `manga` 指的是当前项目 manga-rs，只是一个前端，是一套基于 CLI 交互逻辑的实现。而 `manga-bot` 是一个库([地址](https://github.com/manga-project/manga-bot))，用于提供最核心功能的“模块”。这个模块当前已从 manga-rs 中抽离出来，所以它有独立于 `manga` 的版本。而决定了生成输出文件的结构和平台支持状况的是 `manga-bot` 的版本，而不是 `manga` 的版本，这也是为什么要在版权信息里边写上 `manga-bot` 的版本而不是 `manga` 版本。以后共用 `manga-bot` 模块还会出现 GUI 前端、Android 前端等等（甚至第三方衍生项目），所以强调 `manga-bot` 版本自然至关重要。

## TODO(1.0)

- [x] 基于交互式终端模式
  - [x] 选择平台 -> 选择漫画 -> 保存
  - [x] 漫画索引支持查看更多
  - [x] 漫画保存列表支持多选
- [x] 更多的导出格式支持
  - [x] 基于 epub 转换的 mobi
  - [x] 基于 epub 转换的 pdf
  - [x] 基于 epub 转换的 azw3
  - [x] 基于参数或终端交互定义输出格式
  - [x] 无格式（zip），直接打包原始图片
- [x] 更多的资源来源支持
  - [x] manhua.dmzj.com (动漫之家)
  - [x] www.cartoonmad.com (动漫狂)
  - [x] www.gufengmh.com (古风漫画网)
  - [x] www.hhmmoo.com (汗汗漫画)
  - [x] www.manhuagui.com (漫画柜)
  - [x] www.manhuaren.com (漫画人)
  - [x] www.manhuatai.com (漫画台)
  - [x] www.verydm.com (非常爱漫)
  - [x] www.177mh.net (新新漫画网)
  - [x] e-hentai.org (E-Hentai)
- [x] 其它
  - [x] 清理缓存资源
  - [x] 指定输出目录
  - [x] 原始图片复用（避免重复下载）
- [ ] 接受并处理反馈，将项目重命名为 `manga-cli`
