---
layout: post
title:  "涌动过的emoji，emoji的生与死"
date:   2017-06-17 00:46:18
categories:
- front-end
- canvas
tags:
- front-end
- canvas
excerpt: 这是一个emoji的社会，我是上帝，主宰着emoji的生，与死
published: true
---

> [demo](https://vincentmrlau.github.io/active-background/src/swarming-emoji/)

## 灵感：

![](https://pic3.zhimg.com/v2-f8886ec3fe4899faf685618ceb432676_r.jpg)


看到电脑这么多emoji，今天又是周五，何不搞搞事情？于是就想起了之前那个微笑脸狂涌出来的页面，于是我也搞一个，不过用的不是图片，是emoji的字符，因此可能会存在兼容性的问题😉😉😉

## 开始做事

其实代码非常简单，代码点这里，思路如下：

### 设置一个emoji的类，属性设置如下：

#### 位置信息：

1. x ,y - 坐标：你这个emoji走到哪里要告诉本宝宝吧？

2. vx,vy - 位移信息：我如此逆天怎么可以不知道你这个小emoji往哪里走？

3. size, vSize - 体积信息：小小的emoji一直吃一直吃直到爆炸为止，vSize就是他每天吃的东西了。

4. livingTime - 生存时间：优胜略汰，自然选择。由于我设置的是体积到达一个值的时候就爆炸，跑出边界的就死掉，所以，一代一代下来，最后剩下的就是跑得慢的吃得少的，到最后的最后，一定会静止的。所以每个emoji一定要有一个生存时间，超过这个时间的麻烦你摔下去自己死掉好吗，不要占用生存空间了。

#### 移动+成长+年龄增加的方法，每个时间周期执行一次，岁月更迭，emoji也停不下脚步！

### 设置emoji的涌动的世界的规则

#### emoji涌动世界的emoji数量一定，只有老的死掉新的才能出生（没办法资源有限）

#### 淘汰规则：

1. 跑出了边界：作为一个emoji要有底线！

2. 太大了：emoji要控制自己的饮食，不然会爆炸

3. 太老了：emoji也会自然死亡的

#### 出生规则：

1. 在一定范围内随机：除了年龄是0之外，其他的都是在一定范围内随机

2. 有位置才能出生，占用的是前一个emoji死掉的位置

### 初代emoji出生（new 出来的），生下来就得运动，直至死亡，下一个出生。

## 其他

1. 这样控制emoji的生老病死真是有种造物者的感觉。

2. 除了这个emoji，还用canvas写了好几背景，[传送门](https://github.com/vincentmrlau/active-background)

### 预览
![](https://pic1.zhimg.com/v2-4730b09041505b71586396ea6e7c8be4_r.png)

