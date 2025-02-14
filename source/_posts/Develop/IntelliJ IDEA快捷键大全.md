---
title: IntelliJ IDEA快捷键大全
date: 2025-02-14 17:57:34
categories:
  - IDEA
tags:
  - IntelliJ IDEA
---

IntelliJ IDEA快捷键大全
---

看到一篇 IDEA 快捷键的总结，非常全面，分享一下。

本文参考了 IntelliJ IDEA 的官网，列举了IntelliJ IDEA（Windows 版）的所有快捷键。并在此基础上，为 90% 以上的快捷键提供了动图演示，能够直观的看到操作效果。

该快捷键共分 16 种，可以方便的按各类查找自己需要的快捷键~~

# 一、构建/编译

### `Ctrl + F9`：构建项目

> 该快捷键，等同于菜单【Build】—>【Build Project】
> 

[https://mmbiz.qpic.cn/mmbiz_png/QCu849YTaIM3KqjG0riavLxK8uwjxhusC8x0gyYibF7GLhtcQZn08VxMsCicucS4icSoqjnqqqCPMOkhz04vA8eY6Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1](https://mmbiz.qpic.cn/mmbiz_png/QCu849YTaIM3KqjG0riavLxK8uwjxhusC8x0gyYibF7GLhtcQZn08VxMsCicucS4icSoqjnqqqCPMOkhz04vA8eY6Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

图片

执行该命令后，IntelliJ IDEA 会编译项目中所有类，并将编译结果输出到

```
out
```

目录中。IntelliJ IDEA 支持增量构建，会在上次构建的基础上，仅编译修改的类。

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCT4w7dcr5k9plxQzx6FEuRLBr6x88Wj2w9iaj1cbegfWbZgmVyicw3dOQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCT4w7dcr5k9plxQzx6FEuRLBr6x88Wj2w9iaj1cbegfWbZgmVyicw3dOQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + F9`：重新编译当前类

> 该快捷键，等同于菜单【Build】—>【Recompile ‘class name’】
> 

[https://mmbiz.qpic.cn/mmbiz_png/QCu849YTaIM3KqjG0riavLxK8uwjxhusC7astlQLicGw6XVPADwIl3TO3ZuDSIzpFhBmEfUI0fX6ricdTBkpp9DkA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1](https://mmbiz.qpic.cn/mmbiz_png/QCu849YTaIM3KqjG0riavLxK8uwjxhusC7astlQLicGw6XVPADwIl3TO3ZuDSIzpFhBmEfUI0fX6ricdTBkpp9DkA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

在IntelliJ IDEA 中打开要编译的类，执行该命令会编译当前类。

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC1oicD5axBMicjtWHad4wzEJ8d81lOFAmXUCFLfevkdzFqLdicgoBOORZA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC1oicD5axBMicjtWHad4wzEJ8d81lOFAmXUCFLfevkdzFqLdicgoBOORZA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

# 二、文本编辑

### `Ctrl + X`：剪切

剪切选中文本，若未选中则剪切当前行。

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusClrC3XR7fKiayeQTtCIYibqAhcq0yqhiagpQoSWEIMicLS3ichEu4CEXna2g/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusClrC3XR7fKiayeQTtCIYibqAhcq0yqhiagpQoSWEIMicLS3ichEu4CEXna2g/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + C`：复制

复制选中文本，若未选中则复制当前行。

### `Ctrl + V`：粘贴

### `Ctrl + Alt + Shift + V`：粘贴为纯文本

### `Ctrl + Shift + V`：从历史选择粘贴

从历史剪粘版中选择要粘贴的内容。

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCgabMYrUdXDh69UbRkf2L2xibIlvS3yzZ9MjOkrib1H7ia9zU1C0Xx5FAA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCgabMYrUdXDh69UbRkf2L2xibIlvS3yzZ9MjOkrib1H7ia9zU1C0Xx5FAA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + D`：复制行

复制光标所在行。

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCUBgyzZ01iaibe9aFlJkFMibQKZwRvLWUcWatpQPhQiaG9jc9a35libfz2OA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCUBgyzZ01iaibe9aFlJkFMibQKZwRvLWUcWatpQPhQiaG9jc9a35libfz2OA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Shift + C`：复制文件路径

复制选中文件所在路径。

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCHkpl1vQMeXPp9MH2rN2c6a3fpncib2JZc2r8gNPYpIFvkbnJkpd7ESg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCHkpl1vQMeXPp9MH2rN2c6a3fpncib2JZc2r8gNPYpIFvkbnJkpd7ESg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Alt + Shift + C`：复制引用

复制包的路径，或者类的名称。

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCYapVm0Z0VV0P7CCXYr8hd1moNHjuqXRia9lEicHhL7s7pPlwsY4nlueg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCYapVm0Z0VV0P7CCXYr8hd1moNHjuqXRia9lEicHhL7s7pPlwsY4nlueg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + S`：保存全部

### `Ctrl + Z`：撤销

撤销上一步操作内容。

### `Ctrl + Shift + Z`：重做

恢复上一步撤销内容。

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCUYfcibC28ucUuiaqBRBeNku6lPygY07CPwaMvTzEbKYwwdu26ufIjPQA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCUYfcibC28ucUuiaqBRBeNku6lPygY07CPwaMvTzEbKYwwdu26ufIjPQA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Tab`：缩进

### `Shift + Tabl`：取消缩进

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC6zUSYHDvzywCYjZ6xqfcp0BEWvZ54fOSPicHzXzTXZu0zp5CD3ickuRA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC6zUSYHDvzywCYjZ6xqfcp0BEWvZ54fOSPicHzXzTXZu0zp5CD3ickuRA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Alt + I`：自动缩进行

自动缩进至规范位置。

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCzXIibSSHatbialfjwicNvukQficMndItSXic19UZMDrFtyhQxevNKmm4nXg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCzXIibSSHatbialfjwicNvukQficMndItSXic19UZMDrFtyhQxevNKmm4nXg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Shift + Enter`：开始新行

无论光标是否在行尾，都开始新的行。

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCKvBhia9rbRChRbJWibzmx5eXv39noUEWnjtaISackKiceXPBkIW561NPg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCKvBhia9rbRChRbJWibzmx5eXv39noUEWnjtaISackKiceXPBkIW561NPg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Alt + Enter`：在当前行之前开始新行

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC38nQIqgOOr56POvWXUEdakOUZbSeTLeA2LyiaqSKYHLsqaicNEFAbctw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC38nQIqgOOr56POvWXUEdakOUZbSeTLeA2LyiaqSKYHLsqaicNEFAbctw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Y`：删除行

删除当前行。

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC8Pg43G9bZlW2LoBehSaY4mhvsuEH4lLQtF10jNKMoiac40v0CqnRvZw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC8Pg43G9bZlW2LoBehSaY4mhvsuEH4lLQtF10jNKMoiac40v0CqnRvZw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Shift + U`：大小写转换

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC10STMYElibsogB6yC3L7y9gsyxoCBOZlt24sNu6MrPQZbPebJiauTW4A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC10STMYElibsogB6yC3L7y9gsyxoCBOZlt24sNu6MrPQZbPebJiauTW4A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Alt + Shift + Insert`：创建临时文件

可以创建各种类型的临时文件，该临时文件不会保存到磁盘中。

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCU5pZBk1YJzicuvebcYLAAWVlcXwgwdQUm0P0dOc0dOJosUXse1Vh3vg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCU5pZBk1YJzicuvebcYLAAWVlcXwgwdQUm0P0dOc0dOJosUXse1Vh3vg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Shift + F4`：在新窗口中打开

在新窗口打开当前文件。

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCUrNw6kwT8gcX2AFA7nI8m3PWxgibQwZBTBQ7PtoAibL0w1WS7Rj2GhVA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCUrNw6kwT8gcX2AFA7nI8m3PWxgibQwZBTBQ7PtoAibL0w1WS7Rj2GhVA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

# 三、光标操作

### `Ctrl + Left`：左移一个单词

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCpX9HhBbCBAib626icwia4WwIRibqRl4t7fIiaNHUE4ZXngRFqfhRIWWlIdg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCpX9HhBbCBAib626icwia4WwIRibqRl4t7fIiaNHUE4ZXngRFqfhRIWWlIdg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Right`：右移一个单词

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCFwxODc0q5JoNRxVD3YJeWBL87DLNbnYxQDWJkoVjicyGQiaicUSFRh7NA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCFwxODc0q5JoNRxVD3YJeWBL87DLNbnYxQDWJkoVjicyGQiaicUSFRh7NA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Home`：移动至行首

### `End`：移动至行尾

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCQIfKw81gicaibUhhia6RhicXrfI3WzMmbpMeVVsWCaw5bWSKJ6BRnell3Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCQIfKw81gicaibUhhia6RhicXrfI3WzMmbpMeVVsWCaw5bWSKJ6BRnell3Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Shift + M`：移动至大括号

多次按下快捷键，可以在左右两个大括号间切换。

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCf9vbTCBphk9Tq0b6Oh3rR7wf8IbjVE6LDOtRByXmia25N3uRCK6Jx1Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCf9vbTCBphk9Tq0b6Oh3rR7wf8IbjVE6LDOtRByXmia25N3uRCK6Jx1Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + [`：移动至代码块开始

### `Ctrl + ]`：移动至代码块末尾

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCPj9tajAU6Hta17FrNE2kicWMibn8rxBnYQT1pIpPOeLZysTcrT761a8A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCPj9tajAU6Hta17FrNE2kicWMibn8rxBnYQT1pIpPOeLZysTcrT761a8A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Alt + Down`：下一个方法

### `Alt + Up`：上一个方法

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCYmdLsERruhaD3IUJEtadInv4fZhbBOS2Qib8r5hzviaAnLL5SZgicKKMg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCYmdLsERruhaD3IUJEtadInv4fZhbBOS2Qib8r5hzviaAnLL5SZgicKKMg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + PageUp`：移动至页面顶部

### `Ctrl + PageDown`：移动至页面底部

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCIfxicIicWAiasv0dSm7kaDyBWUib7hiagzQibTicOdDicWRMtMMo60klCQOSzg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCIfxicIicWAiasv0dSm7kaDyBWUib7hiagzQibTicOdDicWRMtMMo60klCQOSzg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `PageUp`：向上翻页

### `PageDown`：向下翻页

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCh9GNDSsrUnoVcn8fyCoetA7BqjsBvMKp4SepGp3ibCVoy1Bwt5vZ0AA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCh9GNDSsrUnoVcn8fyCoetA7BqjsBvMKp4SepGp3ibCVoy1Bwt5vZ0AA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Home`：移动至文件开头

### `Ctrl + End`：移动至文件末尾

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCFaerIOSSGeibfCCeOyia2QyHkNfVLQjiatf4NDOmESDOibsvZqJyOGlCAw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCFaerIOSSGeibfCCeOyia2QyHkNfVLQjiatf4NDOmESDOibsvZqJyOGlCAw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

# 四、文本选择

### `Ctrl + A`：全选

### `Shift + Left`：向左选择

### `Shift + Right`：向右选择

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCa2Ilwh2rE2mDUyweYQolJ1Aq0M9CjDS0czDvI2H9PaeTNbiburCUyQA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCa2Ilwh2rE2mDUyweYQolJ1Aq0M9CjDS0czDvI2H9PaeTNbiburCUyQA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Shift + Left`：向左选择一个单词

### `Ctrl + Shift + Right`：向右选择一个单词

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCTB4ogu8FZmWxcddF0nHd5ibG4fwxDNLnPGZfhd7ia1ziaTdiapArfzkGwA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCTB4ogu8FZmWxcddF0nHd5ibG4fwxDNLnPGZfhd7ia1ziaTdiapArfzkGwA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Shift + Home`：向左选择至行头

### `Shift + End`：向右选择至行尾

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC4leFmmBIN1MuzmJblcaiamht5lVpj4GdBia6kpiasT7YJI0Ew42uhbcxw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC4leFmmBIN1MuzmJblcaiamht5lVpj4GdBia6kpiasT7YJI0Ew42uhbcxw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Shift + Up`：向上选择

### `Shift + Down`：向下选择

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCNBENAyQR1uD9LCncsW8xlnuLCnDAa1dEWD5xxBMjSh1y30kADLwg4A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCNBENAyQR1uD9LCncsW8xlnuLCnDAa1dEWD5xxBMjSh1y30kADLwg4A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Shift + [`：选择至代码块开头

### `Ctrl + Shift + ]`：选择至代码块结尾

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCUkyaCEf6EuYjlicRuOj2vhI3icmoKVbBLQuvMLSg0V95D6MvbWfIeARw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCUkyaCEf6EuYjlicRuOj2vhI3icmoKVbBLQuvMLSg0V95D6MvbWfIeARw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Shift + PageUp`：选择至页面顶部

### `Ctrl + Shift + PageDown`：选择至页面底部

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC6NEXIFoQWZr2ZC9ibDVzb2xzgjpXBY0P8usfbvflUB2Q0PCWBI2XoPQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC6NEXIFoQWZr2ZC9ibDVzb2xzgjpXBY0P8usfbvflUB2Q0PCWBI2XoPQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Shift + PageUp`：向上翻页选择

### `Shift + PageDown`：向下翻页选择

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCehaeQQxrypJlZf086iaChJcHDIia644F7I6WdrGeeK9zFw7a0KqYTrWA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCehaeQQxrypJlZf086iaChJcHDIia644F7I6WdrGeeK9zFw7a0KqYTrWA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Shift + Home`：选择至文件开关

### `Ctrl + Shift + End`：选择至文件结尾

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCIxlHrA7UwEq0iav373AK52giabDoeoJFicO0xXv06pqqR2LJ3aSAEsrmg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCIxlHrA7UwEq0iav373AK52giabDoeoJFicO0xXv06pqqR2LJ3aSAEsrmg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + W`：扩展选择

### `Ctrl + Shift + W`：收缩选择

[data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E](data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

图片

# 五、代码折叠

### `Ctrl + NumPad+`：展开代码块

### `Ctrl + NumPad-`：折叠代码块

[data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E](data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

图片

### `Ctrl + Alt + NumPad+`：递归展开

### `Ctrl + Alt + NumPad-`：递归折叠

[data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E](data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

图片

### `Ctrl + Shift + NumPad+`：全部展开

### `Ctrl + Shift + NumPad-`：全部折叠

[data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E](data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

图片

### `Ctrl + .`：折叠选择

# 六、多个插入符号和范围选择

### `Alt + Shift + Click`：添加/删除插入符号

[data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E](data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

图片

### `Alt + Shift + Insert`：切换列选择模式

[data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E](data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

图片

### 双击`Ctrl` + `Up`：向上克隆插入符号

按`Ctrl`键两次，然后在不松开的情况下按向上箭头键。

[data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E](data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

图片

### 双击`Ctrl` + `Down`：向下克隆插入符号

按`Ctrl`键两次，然后在不松开的情况下按向下箭头键。

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCp24h97TUP6UCwG8G8W4RrXJwqibzAGplo40lt8RYknVaL7ObtOPL32A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCp24h97TUP6UCwG8G8W4RrXJwqibzAGplo40lt8RYknVaL7ObtOPL32A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Alt + Shift + G`：将插入符号添加到选择中的每一行

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC91I0shhEicWzlhaiaj8bWMibAcWcIg4uSkf1TeRGO0AeOgia8Jap8DibFbQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC91I0shhEicWzlhaiaj8bWMibAcWcIg4uSkf1TeRGO0AeOgia8Jap8DibFbQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Alt + J`：选择单位下次出现的位置

### `Alt + Shift + J`：取消最后一次选择

[data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E](data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

图片

### `Ctrl + Alt + Shift + J`：选择所有出现的位置

[data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E](data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

图片

### `Alt + Shift + Middle-Click`：创建矩形选择

[data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E](data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

图片

### `Alt + Click`：拖拽以创建矩形选择区

[data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E](data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

图片

### `Ctrl + Alt + Shift + Click`：拖拽以创建多个矩形选择区

[data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E](data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

图片

# 七、辅助编码

### `Alt + Enter`：显示建议操作

该快捷键又称为“万通快捷键”，它会根据不同的语境建议不同的操作。下面这个演示只是其中的一种，还有很多种用法，你可以尝试一下。

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC9xJpBKicUkonwSEVHT8emQjicJ5KWLWgStHJP8x0hZ26oIxX37HwU6Nw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC9xJpBKicUkonwSEVHT8emQjicJ5KWLWgStHJP8x0hZ26oIxX37HwU6Nw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Space`：代码补全

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC5o3goljlJiamk46aAMAWd8OpOeLWec5rTM920yraMKiciaUyf5RaicxGqQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC5o3goljlJiamk46aAMAWd8OpOeLWec5rTM920yraMKiciaUyf5RaicxGqQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Shift + Space`：类型匹配代码补全

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCnP6WY9bGDFTkujGFicmAE9M0CXe7WmQNRQODaeIBD1bgT4EeIUXJjFA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCnP6WY9bGDFTkujGFicmAE9M0CXe7WmQNRQODaeIBD1bgT4EeIUXJjFA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Alt + Space`：第二次代码补全

[data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E](data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

图片

### `Ctrl + Shift + Enter`：补全当前语句

[data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E](data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

图片

### `Ctrl + Alt + L`：格式化代码

[data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E](data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

图片

### `Ctrl + P`：参数信息提醒

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCqrFTicKzYyibzb6aAS4b8EdWreXAhFZtrtPek3xpnoic6a3ia9OdlwMdDQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCqrFTicKzYyibzb6aAS4b8EdWreXAhFZtrtPek3xpnoic6a3ia9OdlwMdDQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Q`：快速文档

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusClAmc5sG04VxzmKvUubhZaUITicnHhlv4auWibkbDiaAjGn3NUe6yX5hbQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusClAmc5sG04VxzmKvUubhZaUITicnHhlv4auWibkbDiaAjGn3NUe6yX5hbQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Shift + Up`：向上移动语句

### `Ctrl + Shift + Down`：向下移动语句

[data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E](data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

图片

### `Ctrl + Alt + Shift + Left`：向左移动元素

### `Ctrl + Alt + Shift + Right`：向右移动元素

[data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E](data:image/svg+xml,%3C%3Fxml%20version='1.0'%20encoding='UTF-8'%3F%3E%3Csvg%20width='1px'%20height='1px'%20viewBox='0%200%201%201'%20version='1.1'%20xmlns='http://www.w3.org/2000/svg'%20xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg%20stroke='none'%20stroke-width='1'%20fill='none'%20fill-rule='evenodd'%20fill-opacity='0'%3E%3Cg%20transform='translate(-249.000000,%20-126.000000)'%20fill='%23FFFFFF'%3E%3Crect%20x='249'%20y='126'%20width='1'%20height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

图片

### `Alt + Shift + Up`：向上移动队列

### `Alt + Shift + Down`：向下移动队列

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC1wfOfkq0sgXD8Gld3LCjbPNj84ne9oDEJeXsKmzwZjkja5ib3iaqb74Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC1wfOfkq0sgXD8Gld3LCjbPNj84ne9oDEJeXsKmzwZjkja5ib3iaqb74Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + /`：添加行注释

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCydgxWKPkfRB7WmJMsuJUv44gYDZTLlbjiac1mIl4rGoL1wX3wn4libqA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCydgxWKPkfRB7WmJMsuJUv44gYDZTLlbjiac1mIl4rGoL1wX3wn4libqA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Shift + /`：添加块注释

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCeJ6prGJ92T2jxAqX48lKYbLl4dDSxc8GuYGx9Bibw2Q2dCHiaZILrmCg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCeJ6prGJ92T2jxAqX48lKYbLl4dDSxc8GuYGx9Bibw2Q2dCHiaZILrmCg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Alt + Insert`：生产语句

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusChlIJm7v9UYSLUwj9zE0O2qxibeZSMjPRibtlYclz0yXW2X818LwLiapeg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusChlIJm7v9UYSLUwj9zE0O2qxibeZSMjPRibtlYclz0yXW2X818LwLiapeg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

# 八、上下文导航

### `Alt + Down`：跳转至下一个方法

### `Alt + Up`：跳转至上一个方法

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCP62vBcEX0afVVW4sY0O1yarrSXca2ms7Q7Rj4fYZgmfmFKqWQFQpkA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCP62vBcEX0afVVW4sY0O1yarrSXca2ms7Q7Rj4fYZgmfmFKqWQFQpkA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + G`：跳转到指定行

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCEmpNq7FZHBhJDFjCwlHrlbCS4IJAz9MRGicbUTxiaAQMmmvqjdkv0wZw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCEmpNq7FZHBhJDFjCwlHrlbCS4IJAz9MRGicbUTxiaAQMmmvqjdkv0wZw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Tab`：切换活动文件

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCN8zZIusQR2Xu26okcXtTZfWGqzMiaJovxTZnYdBibjb2ZxPN4Zia3IE8Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCN8zZIusQR2Xu26okcXtTZfWGqzMiaJovxTZnYdBibjb2ZxPN4Zia3IE8Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Alt + F1`：选择文件的定位

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCkrrXlMvwa6TZEyemJxvRGlYeJxUvWRs7wHyZU6cjBcWqiaq5WFCF1mg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCkrrXlMvwa6TZEyemJxvRGlYeJxUvWRs7wHyZU6cjBcWqiaq5WFCF1mg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + E`：最近的文件

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCX7XA23pemWRqqUJeYE6cMuyR4XYKP3M9C7FUjgymicNsjE3Pz6LXJSQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCX7XA23pemWRqqUJeYE6cMuyR4XYKP3M9C7FUjgymicNsjE3Pz6LXJSQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Shift + Backspace`：返回上次编辑位置

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCiaSnn3B7sr4s1EvsIRUjMdn7dBg7zRibqJ2Ch6icRzZpRwvU6J6sib3D4Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCiaSnn3B7sr4s1EvsIRUjMdn7dBg7zRibqJ2Ch6icRzZpRwvU6J6sib3D4Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Alt + Left`：后退

### `Ctrl + Alt + Right`：前进

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCeYtPlQUGvKuMyJ8DQmNicvPMOcicD4se3jggxJEgH5BmIwFhrDMmHhvw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCeYtPlQUGvKuMyJ8DQmNicvPMOcicD4se3jggxJEgH5BmIwFhrDMmHhvw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Alt + Down`：下一事件

### `Ctrl + Alt + Up`：上一事件

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC9eSia19sFAMwzvaqxAaUnymdJo1Z3AOEiaic6iaJrvWjniaiaLhTr5GaPRrA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC9eSia19sFAMwzvaqxAaUnymdJo1Z3AOEiaic6iaJrvWjniaiaLhTr5GaPRrA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Alt + Right`：选择下一个选项卡

### `Alt + Left`：选择下一个选项卡

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCRUhibelp6wBS7ib1NgjKEtMQNdPza03vMZYK0aicRRSVsOU6tEdcRofQA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCRUhibelp6wBS7ib1NgjKEtMQNdPza03vMZYK0aicRRSVsOU6tEdcRofQA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `F11`：切换匿名书签

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCP8qXTx7WSYbIPaSjjia7ichNzQcrotmEVdWjXLsLs32bdsmj1rWf3Tfw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCP8qXTx7WSYbIPaSjjia7ichNzQcrotmEVdWjXLsLs32bdsmj1rWf3Tfw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Shift + [digit]`：用数字切换书签

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCmibCuUxmxglAricjTxia4nZNE0WKSXcI567Zibp6Lpp4rMQEpIUqRsBVsA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCmibCuUxmxglAricjTxia4nZNE0WKSXcI567Zibp6Lpp4rMQEpIUqRsBVsA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + F11`：使用助词符切换书签

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCVJW96hjOfCdULb8rqPdOLwYuoPRibx3jqzKo7esaRKUvZvSGhfzRYhg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCVJW96hjOfCdULb8rqPdOLwYuoPRibx3jqzKo7esaRKUvZvSGhfzRYhg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Shift + F11`：显示所有书签

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCoBsyJ43lxNVj6HZao1PGF0xP7BsjAnHLpbJ6n7iaMan6TOf4KAW28hA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCoBsyJ43lxNVj6HZao1PGF0xP7BsjAnHLpbJ6n7iaMan6TOf4KAW28hA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + [digit]`：用数字跳转到书签

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC3YZt19MnGmCYiauaJhWiaeKyjGKiaGqngKeZw1ZTqyv5iaekAKMeoUicr1Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC3YZt19MnGmCYiauaJhWiaeKyjGKiaGqngKeZw1ZTqyv5iaekAKMeoUicr1Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Alt + 7`：显示结构窗口

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCabQrofut1NqkPGHU4iaibRSCNknp5wUX3Ka40FUicTicmH17A3ddDAXMKQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCabQrofut1NqkPGHU4iaibRSCNknp5wUX3Ka40FUicTicmH17A3ddDAXMKQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Alt + 3`：显示查找窗口

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCemJMFDfeGbtmATLKxrQ4O8iasQ458HAQIiciaib0u3Fq4oJ4iaOkwyTQbjg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCemJMFDfeGbtmATLKxrQ4O8iasQ458HAQIiciaib0u3Fq4oJ4iaOkwyTQbjg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

# 九、查找操作

### 双击`Shift`：查找所有

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCiaw2EotlcgB5v0dLKzRxchKTRYHdYiaDN6GTC1O8ia9bW6cNltJF5aJBQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCiaw2EotlcgB5v0dLKzRxchKTRYHdYiaDN6GTC1O8ia9bW6cNltJF5aJBQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + F`：查找字符（当前文件）

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCZyTbuhP4pdclD7WVO5qGKl21MPy1cYqUE5LBick9fagqpPiaHd8t1eug/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCZyTbuhP4pdclD7WVO5qGKl21MPy1cYqUE5LBick9fagqpPiaHd8t1eug/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `F3`：查找下一个

### `Shift + F3`：查找上一个

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCpdeaSKeuAiaVad1NeO2082r58HppuVIkvs2ImP96ic0MQrNhZibhcI4cA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCpdeaSKeuAiaVad1NeO2082r58HppuVIkvs2ImP96ic0MQrNhZibhcI4cA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + R`：替换字符（当前文件）

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCALWibrchqFlzqNphB8ia1LMW3zNicLichJS3wrjVmibfiauBdXOic4KquWmvw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCALWibrchqFlzqNphB8ia1LMW3zNicLichJS3wrjVmibfiauBdXOic4KquWmvw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Shift + F`：查找字符（所有文件）

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCIEoqtAnX3HjA02qFQjrNLM4gjQmAiaMLVy9RaWG5g0Ob5yXHgf0EQaA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCIEoqtAnX3HjA02qFQjrNLM4gjQmAiaMLVy9RaWG5g0Ob5yXHgf0EQaA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Shift + R`：替换字符（所有文件）

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCEXwEdxpbRFWwZXWSflRibyyEjooY5Da5GJQbsc9ziaSMLu2O2Cgjf5AQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCEXwEdxpbRFWwZXWSflRibyyEjooY5Da5GJQbsc9ziaSMLu2O2Cgjf5AQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + F3`：跳转到光标处单词的下一位置

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCC4l90YSS8GsMNpUQ98UWVy78YU8RIujQUK37ibWPprkUeCbS4cJkBJA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCC4l90YSS8GsMNpUQ98UWVy78YU8RIujQUK37ibWPprkUeCbS4cJkBJA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Shift + N`：查找文件并跳转

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusChLS9NIMA8icorQcao4GNvBPibgtLC0NKXBWxwUJnLLqWIndTNdeMORZw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusChLS9NIMA8icorQcao4GNvBPibgtLC0NKXBWxwUJnLLqWIndTNdeMORZw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + F12`：打开文件结构

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCFQEia6IWykBXq7ASp921ibzCmeUAd0KUERVFic69icAErUZLiaMRrf007Yw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCFQEia6IWykBXq7ASp921ibzCmeUAd0KUERVFic69icAErUZLiaMRrf007Yw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Alt + Shift + N`：查找符号（变量、方法等）

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCe7n3E6C8mDCxvYm5bsGIXo9EmWDZvch0hqqOBxwkdViaqKmTDx0yraQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCe7n3E6C8mDCxvYm5bsGIXo9EmWDZvch0hqqOBxwkdViaqKmTDx0yraQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Shift + A`：查找动作

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCQZCHvPpaJ0dhAoVNiclZxASBIKvFVDsaJYb1faRlDlbica4YmZ0oht2A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCQZCHvPpaJ0dhAoVNiclZxASBIKvFVDsaJYb1faRlDlbica4YmZ0oht2A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

# 十、符号导航

### `Alt + F7`：查找用法

### `Ctrl + B`：跳转到声明处

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCUtzcWMjFCibWeumreI2gNLZKnbBEla5rgpqOo337icg1PWFkTKaKYT3Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCUtzcWMjFCibWeumreI2gNLZKnbBEla5rgpqOo337icg1PWFkTKaKYT3Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Shift + B`：跳转到声明类处

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCGNxU7aAEpKcqUUCburvicSLqZV0uuO7dFLpNGjw7auN4nYGqKoic028A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCGNxU7aAEpKcqUUCburvicSLqZV0uuO7dFLpNGjw7auN4nYGqKoic028A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Alt + F7`：显示用法

### `Ctrl + U`：跳转到超级方法

### `Ctrl + Alt + B`：跳转到实现方法

[https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCynI3KDzPAaYPaia5fyibAUB2l3qdLoLpAVFAcwKPqJdVvDcdTVibvpiaeg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCynI3KDzPAaYPaia5fyibAUB2l3qdLoLpAVFAcwKPqJdVvDcdTVibvpiaeg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

图片

### `Ctrl + Shift + F7`：突出显示文件中的用法