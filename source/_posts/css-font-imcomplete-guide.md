---
title: CSS字体不完全指南
date: 2020-05-27 17:49:41
categories:
  - 前端
  - CSS
tags:
  - 前端
  - CSS
  - Font
---

本文主要介绍网页 CSS 使用字体的规则, 便于前端和设计理解与沟通 : )

# 字体集

> 场景: 设计师验收时说这个要什么字体, 这个应该是什么字体. 那前端是要一个个修改指定字体吗?<br>
> No! 一个全局 font-family 就搞定了.

全局设置`font-family`可以为整个项目指定一系列适配字体.
按先英文字体后中文字体的顺序, 就会按照优先匹配原则选用中英文字体.
按此顺序, 遇到英文时会先匹配到英文字体, 遇到中文时因为英文字体中大多数不包含中文, 就会依次往后匹配中文字体.

⚠️ 现在设计的使用规则是, 英文、数字用自选字体 FFDIN, 中文有苹果系统有的 PingFang SC (注意中间有空格), 建议可以再定一个其他系统用的中文字体

```JavaScript
// 字体集
font-family: AxFFDIN, Helvetica, Arial, 'PingFang SC', sans-serif;

//解释
font-family: AxFFDIN // 英文字体 自选字体, 通过@font-face指定在线字体
                       Helvetica // 英文字体 苹果系统自带
                       Arial  // 英文字体 其他系统
                       'PingFang SC' // 中文字体 苹果系统自带 缺少其他系统的中文字体
                       sans-serif;  // 最终fallback, 无衬线字体 字体族名称
```

# 字重

> 场景: 设计师验收时说这个字体是粗体, 设计稿上是 PingFangSC-Medium, 这时前端将字体改成苹方字体中的 medium 对吗?<br>
> No!按照之前 font-family 介绍中的匹配原则, 可能用户系统没有苹方字体, 更别说某个 Medium 了, 这里应该用到的是 font-weight 字重.

CSS`font-weight`属性值有两种, 数值和名称, 数值为 100~900 的 9 种, 与名称大致对应.
很少存在一种字体有 9 个字重, 完全匹配 CSS 字重属性的 通常字体拥有的字重数量为 4~6 个, 不过 400 (normal) 和 700 (bold)基本上是必备的.

没有对应字重时, 字体匹配规则如下:
- 字重小于 400 的, 会先降序匹配, 找不到再升序匹配
- 字重大于 500 的, 会先升序匹配, 找不到再降序匹配
- 字重 400, 500 的会优先互相匹配, 再执行第一条规则

这也就是有时候改变 CSS 字重属性, 字体看起来却没变的原因. CSS 字重属性最终还是要匹配到字体的字重的

insight: 推荐使用 CSS 字重的数值, 而不是指定具体字重的字体, 如:PingFangSC-Medium, 这样就可以在匹配一套字体集的基础上匹配字重

| 数值 | 名称                      | Helvetica Neue           | PingFang SC           |
| -- | -- | ----------- | ----------- |
| 100  | Thin                      | HelveticaNeue-UltraLight | PingFangSC-Ultralight |
| 200  | Extra Light (Ultra Light) | HelveticaNeue-Thin       | PingFangSC-Thin       |
| 300  | Light                     | HelveticaNeue-Light      | PingFangSC-Light      |
| 400  | Normal                    | HelveticaNeue-Regular    | PingFangSC-Regular    |
| 500  | Medium                    | HelveticaNeue-Medium     | PingFangSC-Medium     |
| 600  | Semi Bold (Demi Bold)     | -                        | PingFangSC-Semibold   |
| 700  | Bold                      | HelveticaNeue-Bold       | -                     |
| 800  | Extra Bold (Ultra Bold)   | -                        | -                     |
| 900  | Black (Heavy)             | -                        | -                     |

<center>字重数值、名称与两款常见字体的对应关系</center>

# 参考链接

[如何优雅的选择字体](https://segmentfault.com/a/1190000006110417)
[字重](https://keqingrong.github.io/blog/2019-06-11-font-weight)
[深入了解 font-weight](https://aotu.io/notes/2016/11/08/css3fontweight/index.html)
