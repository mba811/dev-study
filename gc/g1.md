# NPAPI 插件无法在 Chrome 42 版及更高版本上正常运行

您可以利用插件在浏览器中添加一些额外的功能。例如，您可以观看某些类型的视频或者玩网页版游戏。

### NPAPI 支持已结束

过去，许多插件都是使用一种称为 NPAPI 的旧系统开发的。如今，只有少量网站在使用 NPAPI 插件，因为这些插件有时会给网站带来安全风险。

为了让用户获得更安全、更快速且更稳定的 Chrome 浏览体验，我们[已结束 Chrome 42 版对 NPAPI 插件的支持](http://blog.chromium.org/2014/11/the-final-countdown-for-npapi.html)。

### 支持哪些插件

使用 Pepper API (PPAPI) 这种更新、更安全的系统的插件将继续正常使用，包括 Chrome 自带的那些插件，如 Adobe Flash 和 PDF 查看器。但是，一些使用 NPAPI 的插件（包括 Silverlight、Java 和 Unity）将无法使用。

### 如何临时启用 NPAPI 插件

如果您必须使用 NPAPI 插件，​​可以采用下面介绍的临时解决方法（在 Chrome 45 版于 2015 年晚些时候发布之前，此方法将一直有效）：

  1. 打开 Chrome。
  2. 在屏幕顶部的地址栏中，输入 chrome://flags/#enable-npapi
  3. 在随即打开的窗口中，点击**启用 NPAPI** 标记下方显示**启用**的链接：

![Enable NPAPI Flag Screenshot](https://lh3.googleusercontent.com/Z19NgVGnekvaEoP087Xjs7yiQg3ZqmAhjz-DzKAcvzvl-n76gxkdxYKRgqN7sw=w434-h126)

  4. 点击页面左下角的**立即重新启动**按钮。

Chrome 45 版本发布后，您将需要使用其他网络浏览器来加载需要 NPAPI 插件的内容。
