---
title: EPSG 4326 vs EPSG 3857 (投影，数据集，坐标系统等等)
author: pen
type: post
draft: false
pubDatetime: 2024-04-07T17:43:46+08:00
lastmod: 2024-04-07T17:44:27+08:00
url: /2024/04/07/epsg-4326-vs-epsg-3857-(投影，数据集，坐标系统等等)
description: 如果你想了解EPSG:4326（WGS 84）和EPSG:3857（Web Mercator）之间的区别、数据、投影以及坐标系统的更多信息，这篇文章通过易懂的草图和详细解释，为你提供了答案。
featured: false
postSlug: data-map-coordinate
tags:
  - epsg-4326
  - epsg-3857
  - epsg
  - 900913
  - 投影
  - 数据集
  - 坐标系统
---
> 原文链接 [ EPSG 4326 vs EPSG 3857 (projections, datums, coordinate systems, and more!) ](http://lyzidiamond.com/posts/4326-vs-3857 "EPSG 4326 vs EPSG 3857 (projections, datums, coordinate systems, and more!)")


太长不看版：如果你想知道 EPSG: 4326 和 EPSG:3857 之间的区别，可以看一下这篇文章。如果你想知道更多的关于数据，投影以及坐标系统，这篇文章会给你答案。如果你喜欢蠢蠢的草图，也可以阅读这篇文章。

额外说明：这最开始是一篇写给我 mapbox 同事看得开发日志，所以如果里面有一些词或者语法感到奇怪，我不能保证百分之百的精准。

我们可以用地图做很多很酷的事情，但是只有一个是它不可或缺的特性：地图提供了一种我们和位置进行连接的通用框架。如果没有这个通用框架，地图将不会变得这么易用—举个例子，比如如果没有地图两地之间的距离测量几乎不可能。

但是地图不仅仅提供了一个简单的连接我们与位置和测距的系统 — 确切的说是地球表面的距离测量和与人的位置连接。有一个被称为坐标系统的概念，会告诉你地图不仅仅是我们看到的那样的，还会告诉我们数据是怎么存储的，距离是怎么测量的。
![](https://ws1.sinaimg.cn/large/006tKfTcgy1frb6dzdcw5j31kw0wo4qt.jpg)

在 web/mobile  地图中，我们主要使用两个坐标系统 **EPSG: 4326(WGS 84)** 和 **EPSG: 3857 (Web Mercator)** 。这篇文章将会告诉你什么是坐标系统，为什么会有这两个系统， 以及怎么理解它们之间的差异。

## GeoDesy （大地测量学）
![](https://ws4.sinaimg.cn/large/006tKfTcgy1frb6e0y8e0j31kw1awx6r.jpg)
测量地球的学科叫做 geodesy （中文叫大地测量学）。以下是大地测量学的一个简单介绍：
1. 与我们通常的描述不同，地球不是一个球体，也不是一个球状体，甚至也不是一个椭球体。虽然使用这样的近似非常有用，但是事实上地球是一个在四维空间中旋转的奇怪的物体，而且也是由多层不同密度组成，导致它非常的不容易被模型化。
	** 地球真实的形状太过复杂，使用它自己本身作为参考对于测量来说太难 **
2. 于是，我们使用一个参考椭球体来对地球表面进行模拟，一个参考椭球体（reference ellipsoid）是一个数学上定义的去掉了地表特征的球体（geoid — 真实的包含地表信息的模型）
 ![](https://ws4.sinaimg.cn/large/006tKfTcgy1frb6e2qhl3j31kw13yqv8.jpg)
3. 但是其实并没有一个权威的椭球体统一的用来标准化每一个位置。因为一个参考椭球体是一个近似值，在有的椭球体中，一些位置可以精准的表示出来，而在有些椭球体中则相差千里（因为他们的地表形态的不同）。在一些位置上，一个完全不同的参考椭球体比在别的位置使用的参考椭球体更加适用。
![](https://ws1.sinaimg.cn/large/006tKfTcgy1frb6e437a7j31kw0pp4qr.jpg)
## 坐标系统
通过定义一个参考椭球体，我们现在终于能有一个东西来承载我们的地理坐标系统了。一个地理坐标系统通过一些可以测量的点来描述在这个椭球体表面的位置。这项工程非常的巨大 — 你可以想象尝试这个告诉某个人你的家，不使用英里，米，或者其他的基础度量单位，或者不使用平方米来描述房屋的面积。一个坐标系统也是这样的一个标准 — 用来标准描述地图上位置。通常的理解就是这个东西允许人们在外对这个世界进行探索的时候，能都轻松的找到对应的位置通过人们以往描述的数据。
![](https://ws3.sinaimg.cn/large/006tKfTcgy1frb6e55onyj31kw0nbu0z.jpg)
有两种类型的坐标系统：地理学坐标系统和投影坐标系统

## 地理学坐标系统（Geographic coordinate systems）
地理学坐标系统使用三维建模构建一个椭球体，同时将椭球体表面进行删格化并在这个表面上标注位置。当我们在描述位置时使用的是『longitude』(东/西经)和『latitude』(南/北纬), 我们就是在使用地理学坐标系统

数据集（Datums），从另一方面来说，就是地理学坐标系统构建在一个典型的椭球体上（有如此之多的地理坐标系统），标注一个特定的位置，还有自己特定的朝向。它们也被称为『空间参考系』或者『坐标参考系』。数据集（Datums）是如此的重要：每一个地图和空间数据集都有一个，并且同时有如此之多的数据（Datums）被广泛的应用在地球上不同的位置。有一些数据集（Datums）能精准的描述这个世界的中心，因为这些参考椭球体对这些位置进行了非常准确的匹配，但是在一些别的地方可能就相差千里了。
幸运的是，如同我下面所说，你几乎不用担心怎么处理这些数据，因为现在已经有很多软件可以处理这些数据集了。
![](https://ws3.sinaimg.cn/large/006tKfTcgy1frb6e6a5bej31ha19wx6q.jpg)

再次重申，创建一个通用的系统来描述地理位置非常的重要，只有有了一个这种系统才能做之后的处理。想要了解更多，可以到 National Geodetic Survey 上去找一篇这个文章看一下[视频地址](https://www.ngs.noaa.gov/corbin/class_description/NGS_Datums_vid1/) 。非常推荐。

## 投影坐标系统
投影坐标系统有点像地理学坐标系统，但是相比而言省略了一步。投影坐标系统也是一个对地球各地位置进行栅格处理的系统，但是它会把三维的栅格拍平到一个二维的平面上（比如一张纸质地图或者屏幕）。
> 一个便于理解的可视化投影坐标系统的方案是，在这个参考椭球体的中心放置一个光源，并且同时使用一张纸将这个椭球体包裹，无论是裹成一个圆锥体，圆柱体，或者什么其他的形状。每一个坐标都会在这张纸上有一个投影，然后当你打开这张纸，并且放置在桌面上，这些坐标点就是你的投影数据

一个投影坐标系统就是定义了如何将一个三维的模型转换为一个二维的平面（这样相比于地球仪更容易携带），这个在数学上被称为投影。
![](https://ws1.sinaimg.cn/large/006tKfTcgy1frb6e83spsj31kw18fqv8.jpg)
下面的链接是一个非常经典的介绍地图投影的课程[The West Wing](https://www.youtube.com/watch?v=eLqC3FNNOaI "The West Wing "), 如你之前没有看过这个视频，现在就可以看下，如果你之前已经看过了，现在可以在看一遍。

## 好了，那么什么是 Web 地图呢？
几乎所有的 Web 地图都是采用基于上面两种坐标系统的 EPSG: 4326（WGS84）和 EPSG:  3857（Pseudo-Mercator）。无论你是否知道自己在用这两种坐标系统，你只要和在线地图交互就不可能离开这两个坐标系统。

> 关于它们名称的说法：坐标系统（地理学或者投影）都会用 [EPSG](https://epsg.io/) 作为标志。EPSG 是 European Petroleum Survey Group 的缩写，这是一个致力于将大地测量学的研究和应用做到最完美的组织。 2005 年的时候， EPSG 被吸纳进 International Association of Oil & Gas Producers (IOGP) 组织， 但是坐标系统的支持部门还是叫做 EPSG Geodetic Parameter Dataset。
![](https://ws2.sinaimg.cn/large/006tKfTcgy1frb6e99ak4j31kw0ve4qq.jpg)

### EPSG:4326 (WGS84)
世界大地测量系统1984 （World Geodetic System of 1984) 是 GPS 用来描述地球上位置的地理学坐标系统（三维）。WGS84 通常使用 GeoJSON 作为坐标系统的单位，GeoJSON 中使用数字作为经度和纬度的单位。大部分时候，当你描述一个经纬度坐标的时候，它就是基于 EPSG:4326 坐标系统的。这也是我们在 Mapbox 中储存数据的方式。
![](https://ws4.sinaimg.cn/large/006tKfTcgy1frb6eai6ctj30sa1o0qv6.jpg)
我们没有办法在二位平面上展示 WGS84 坐标系统，所以大部分的软件在展现这种坐标的时候都会使用一个叫做 equirectangular （EPSG:54001）的投影（即直接使用经纬度单位）
	Equirectangular projection(ERP)是一种简单的投影方式，将经线映射为恒定间距的垂直线，将纬线映射为恒定间距的水平线。这种投影方式映射关系简单，但既不是等面积的也不是保角的，引入了相当大的失真。

### EPSG: 3857 (Pseudo-Mercator)
Pseudo-Mercator 投影系统将 WGS84 坐标系统投影在平面上（这个投影规则也被称之为球面墨卡托或者 web 墨卡托）。但是这个投影系统并不是包含地球上所有的位置，北纬和南纬的85.06度以上的地区不会展示。这个投影首次是被使用在 Google 地图上，加上几乎所有的 Web 地图，但是有趣的一点是，这些投影（EPSG:3857）内部都是使用的 WGS84 坐标系统 -- 即使用的 WGS84 椭球体构建，但是将它们（EPSG:3857）的坐标是投射在一个球面上。

通过这个投影规则可以投射出的正方形的地图，但是如果想将两个不同的参考椭球体投影在同一个投影坐标系上是不对的，这就表示在软件上必须展示是动态可变的。当在软件上是可变的，那我们在软件中就不能对一个位置得出稳定的坐标。这些意味着 EPSG:3857 对于计算机而言是一个非常好的用来展示的坐标系统，但是不是一个稳定的可以用来分析存储数据的坐标系统。（这也是为什么 mapBox 在存储数据的时候使用的是 EPSG: 4326 但是展示的时候使用 EPSG:3857）。
![](https://ws4.sinaimg.cn/large/006tKfTcgy1frb6ecc0pvj31kw184x6s.jpg)
如果你想知道更多的关于 EPSG: 3857 的数学运算规则，可以看这篇文章 。
[EPSG:3857 的数学运算](https://alastaira.wordpress.com/2011/01/23/the-google-maps-bing-maps-spherical-mercator-projection/ "EPSG:3857 的数学运算")

###  Web 地图数据
大部分时候， Web 地图在存储数据的时候都是使用的 WGS84 的坐标系统（在有些系统中，这个规格被称之为「未投影」的数据）并且在可视化的时候使用EPSG:3857 (Pseudo-Mercator)。但是有些时候，一个地图开发者可能会说他们想看一下 WGS84 的坐标系统（或者将这个描述为 EPSG:4326）。如上面所说， WGS84 是一个未投影的数据 — 在二维平面上是无法展现这个坐标系统的。如果有人说他们想看 WGS84 的数据，正确的做法是只能看到 Plate-Carrée 投影的数据 — Plate-Carrée 本质上就是将角度平铺在平面上。
![](https://ws1.sinaimg.cn/large/006tKfTcgy1frb6ee4h3bj31kw19pu10.jpg)
![](https://ws1.sinaimg.cn/large/006tKfTcly1frb6inn8dhj31kw19hb2d.jpg)

在 Wikipedia 上有一些使用  Platte-Carré 投影的有意思的历史：
> 十分特别的是，由于地图上的图像像素的位置与其在地球上相应的地理位置之间的特别简单的关系，plate carrée 已经成为全球地图数据集的标准，例如Celestia和NASA World Wind。


## 更多相关资料可以看以下链接

+ [ArcGIS Desktop help](http://help.arcgis.com/en/arcgisdesktop/10.0/help/index.html#//00v20000000q000000.htm)
+ [Datums on Wikipedia](https://en.wikipedia.org/wiki/Geographic_coordinate_system#Datums)
+ [WGS on Wikipedia](https://en.wikipedia.org/wiki/World_Geodetic_System)
+ [IOGP page on “practical geodesy”](http://www.iogp.org/geomatics/#practical-geodesy)
+ [Understanding Projections by Tom MacWright](https://macwright.org/2012/01/27/projections-understanding.html)
+ [The Wild World of Geodesy! Maptime slides](http://lyzidiamond.com/geodesy/)
