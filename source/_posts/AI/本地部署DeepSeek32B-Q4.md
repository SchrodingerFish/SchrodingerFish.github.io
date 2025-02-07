---
title: 本地部署DeepSeek32B_Q4
date: 2025-02-07 15:46:59
categories:
  - AI
tags:
  - AI
  - DeepSeek 
  - Ollama
---
{% note warning %}
转自[潘同学的笔记](https://zhuanlan.zhihu.com/p/21030210489),如有侵权,请联系我删除.
{% endnote %}
> **由中国深度求索打造的[DeepSeek](https://zhida.zhihu.com/search?content_id=253228484&content_type=Article&match_order=1&q=DeepSeek&zhida_source=entity)在全球掀起"中国智造"风暴！这款革命性应用不仅强势包揽中美双料App Store免费榜冠军，更以单日下载量超ChatGPT四倍的惊人战绩横扫全球市场。仅凭500万美元研发投入，便实现对[OpenAI](https://zhida.zhihu.com/search?content_id=253228484&content_type=Article&match_order=1&q=OpenAI&zhida_source=entity)、谷歌等硅谷巨头数十亿美元级AI项目的全面超越，用硬核技术实力诠释"中国式创新"效率！这场以小博大的科技逆袭，不仅刷新全球AI产业格局，更让世界见证[东方智慧](https://zhida.zhihu.com/search?content_id=253228484&content_type=Article&match_order=1&q=%E4%B8%9C%E6%96%B9%E6%99%BA%E6%85%A7&zhida_source=entity)的破局力量。**

由于DeepSeek的火爆，用户访问过多，导致官方服务器多次[宕机](https://zhida.zhihu.com/search?content_id=253228484&content_type=Article&match_order=1&q=%E5%AE%95%E6%9C%BA&zhida_source=entity)，这十分影响客户的体验性。

**这里将教大家如何在本地部署DeepSeek,让大家可以随时随地使用DeepSeek。**

**事先扫盲：**

**我们本地部署的[大模型](https://zhida.zhihu.com/search?content_id=253228484&content_type=Article&match_order=1&q=%E5%A4%A7%E6%A8%A1%E5%9E%8B&zhida_source=entity)都不是DeepSeek，而是蒸馏后的模型，想要部署真实的r1或者v3，一般都是企业级的应用了，因为完整版模型大小需要占用1.3T的显存，消费级显卡无法单独运行，必须由企业显卡集群部署。**

**1.[Ollama](https://zhida.zhihu.com/search?content_id=253228484&content_type=Article&match_order=1&q=Ollama&zhida_source=entity) 下载安装**

Ollama 是一个轻量级的本地AI模型运行框架，可在本地运行各种开源[大语言模型](https://zhida.zhihu.com/search?content_id=253228484&content_type=Article&match_order=1&q=%E5%A4%A7%E8%AF%AD%E8%A8%80%E6%A8%A1%E5%9E%8B&zhida_source=entity)（如Llama、[Mistral](https://zhida.zhihu.com/search?content_id=253228484&content_type=Article&match_order=1&q=Mistral&zhida_source=entity)等）

浏览器输入网址：[https://ollama.com/](https://link.zhihu.com/?target=https%3A//ollama.com/)

选择一个系统进行下载

![](https://pica.zhimg.com/v2-a71cfc9444edae94eed9560333989272_1440w.jpg)

![](https://pic4.zhimg.com/v2-754144fe4409cb1adee66c9c5c013767_1440w.jpg)

安装之后，Ollama已经运行了，它是CMD命令工具，我们可以在命令行输入ollama来验证，是否安装成功

如果出现下图的内容的话，就表示下载成功

![](https://pic4.zhimg.com/v2-82768b9ac3bc5b34347ce298f2a2e43d_1440w.jpg)

**安装DeepSeek-r1模型** 还是在刚才的Ollama网站，选择Model模块，选择deepseek-r1这个模型

![](https://pic1.zhimg.com/v2-d9f0318c9ca76ed0410f62aac88562fe_1440w.jpg)

![](https://pic2.zhimg.com/v2-8f9061b1f586f8f1c7c3122c7eb78c01_1440w.jpg)

搜索出来有很多个版本，区别就是参数不一样。

1.5b，7b，8b，14b，32b，70b，671b

每个版本对应所需的内存大小都不一样。

如果你电脑[运行内存](https://zhida.zhihu.com/search?content_id=253228484&content_type=Article&match_order=1&q=%E8%BF%90%E8%A1%8C%E5%86%85%E5%AD%98&zhida_source=entity)为8G那可以下载1.5b，7b，8b的蒸馏后的模型

如果你电脑运行内存为16G那可以下载14b的蒸馏后的模型

我这里选择14b的模型，参数越大，使用DeepSeek的效果越好

使用ollama run deepseek-r1:14b 进行下载

![](https://pic2.zhimg.com/v2-a9081f6db9305b2f5b1c0d97ac06eac1_1440w.jpg)

![](https://pic3.zhimg.com/v2-ecb01e4f41acf536c41168ca8049c2a2_1440w.jpg)

这里我第一次失败了，所以我试了两次，才成功。

我们也可以在命令行中输入，ollama list 查看是否成功下载了模型，下图的内容表明下载成功

![](https://pic3.zhimg.com/v2-37bbf60de0f14838fb6a3962f7dca85c_1440w.jpg)

输入ollama run deepseek-r1:14b运行模型

启动成功后，就可以输入我们想问的问题，模型首先会进行深度思考（也就是think标签包含的地方），思考结束后会反馈我们问题的结果。

![](https://pica.zhimg.com/v2-98b93c30895f1db3159eecb01f9e69fe_1440w.jpg)

**注意：由于ollama默认是将模型下载到C盘的，如果你的C盘空间也和我的一样拉跨**

![](https://pic4.zhimg.com/v2-375e98afbb0de7e7d1997711898af857_1440w.jpg)

**那我们就需要更改一下模型的下载位置了**

**编辑环境变量**

![](https://pic2.zhimg.com/v2-7b7771ad22581145d0271b2e5f515c25_1440w.jpg)

![](https://pic2.zhimg.com/v2-6b1fbe1b839f047e5be883f9252dc2af_1440w.jpg)

**新建一个[系统环境变量](https://zhida.zhihu.com/search?content_id=253228484&content_type=Article&match_order=1&q=%E7%B3%BB%E7%BB%9F%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F&zhida_source=entity)**

![](https://pic2.zhimg.com/v2-fe8d0d297cafe73015b501d5f3c708af_1440w.jpg)

![](https://pica.zhimg.com/v2-e3781a9689d7e05de83825e13a9012e8_1440w.jpg)

**变量名请一定写[OLLAMA](https://zhida.zhihu.com/search?content_id=253228484&content_type=Article&match_order=1&q=OLLAMA&zhida_source=entity)\_MODELS**

**然后变量值，就写你模型下载所下载到的路径**

**然后还需要去用户环境变量里，新增两个环境变量**

**OLLAMA\_HOST：0.0.0.0**

**OLLAMA\_ORIGINS：\***

![](https://picx.zhimg.com/v2-f6e91e9267bbcedc65766170cdb3dd3f_1440w.jpg)

![](https://pic1.zhimg.com/v2-da4418f86e0a977315df7a24e939dc50_1440w.jpg)

**都设置好后，就保存，然后重启ollama**

**2.chatBox下载安装**

[Chatbox AI官网：办公学习的AI好助手，全平台AI客户端，官方免费下载](https://link.zhihu.com/?target=https%3A//chatboxai.app/zh)

我们通过ollama下载模型后，可以在命令行使用deepseek了，但是命令行的形式还是有些不美观，所以我们可以借助chatBox,它拥有美观的UI，只要接入ollama的[Api](https://zhida.zhihu.com/search?content_id=253228484&content_type=Article&match_order=1&q=Api&zhida_source=entity)就可以使用了。

也是可以多平台下载，我这里选择windows下载

![](https://pic3.zhimg.com/v2-61995510606a20a413ea6802792bad3a_1440w.jpg)

安装启动后，点击设置

![](https://pic3.zhimg.com/v2-83ae34245dbec3286efaff5d02117cea_1440w.jpg)

模型提供方选择ollama [API](https://zhida.zhihu.com/search?content_id=253228484&content_type=Article&match_order=1&q=API&zhida_source=entity)

![](https://picx.zhimg.com/v2-8108b12ff9c67882b4a900512f6012fb_1440w.jpg)

然后现在可以选择模型了

![](https://pic3.zhimg.com/v2-9108eb961c7e5696bb00244856372842_1440w.jpg)

能成功使用

![](https://pic2.zhimg.com/v2-e24c3954a0eda7c6071c8ff4ad74cc21_1440w.jpg)

**注意：选择[蒸馏模型](https://zhida.zhihu.com/search?content_id=253228484&content_type=Article&match_order=1&q=%E8%92%B8%E9%A6%8F%E6%A8%A1%E5%9E%8B&zhida_source=entity)的大小，需要结合自己的电脑实际情况来选择，这会关系到，模型回答的速度以及效果问题。你也可以选择用[cherry-studio](https://cherry-ai.com)作为客户端，配置方法同上面类似**

**最后我想说：**

特斯拉的颠覆性创新，从来不只是硅谷实验室的灵光乍现，而是整个北美科技森林共生共荣的果实。他们总能在技术拐点到来前三年埋下种子，用工程师文化浇灌创新萌芽。中国人工智能的破局，正需要这样的生态雨林。

太多国产操作系统止步于实验室，恰因缺少开源的阳光雨露，只能做技术潮流的追随者。因此，中国必须要有自己的数字原住民，在代码荒原上开垦出开源社区。

我始终相信技术平权的力量，就像三十年前没人能想象农民直播卖货，而今天数字技术已渗入每个乡村角落——（当世界还在争论AI伦理时，我们的AI工程师正在帮陕西老农用大模型预测苹果期货价格！这才是真正值得自豪的突破）

终有一天我们能看到，敦煌壁画修复师用生成式AI重现盛唐金碧，山区小学教师和上海陆家嘴的量化交易员共享同一套算法底座。

最后愿星辰与代码同辉，愿最前沿的科技，扎根在最烟火的人间。