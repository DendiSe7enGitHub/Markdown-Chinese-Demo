---
layout: post
date:   2018-05-20
desc: "How to create your own Theme for Vuepress"
keywords: "Tutorial,Jekyll,gh-pages,website,blog,Git"
categories: Vue
tags: [Vue,Vuepress]
name: README
coverimg: "http://ww1.sinaimg.cn/large/88b26e1cgy1frpq5olvaqj20sg0lc126.jpg"
info: "How to create your own Theme for Vuepress"
---

# License
![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)


# 导航
- [License](#license)
- [导航](#导航)
- [Vuepress项目结构](#vuepress项目结构)
- [流程及VuePress源码解读](#流程及vuepress源码解读)
    - [仓库地址](#仓库地址)
    - [[config.js] 配置文件说明](#configjs-配置文件说明)
        - [[config.js]所在的目录](#configjs所在的目录)
        - [[config.js] 中的坑](#configjs-中的坑)
            - [我们传入的是什么?](#我们传入的是什么)
            - [传入的值经过了怎么样的处理?](#传入的值经过了怎么样的处理)
            - [我们所拿到的值是什么?](#我们所拿到的值是什么)
    - [如何定义我们自己的流程](#如何定义我们自己的流程)
        - [`package.json`里面关于`script`的配置](#packagejson里面关于script的配置)
        - [`doc`用来干嘛呢?你可能会有这样的问题](#doc用来干嘛呢你可能会有这样的问题)
- [结语](#结语)
  
# Vuepress项目结构

``` bash

├── bin      		构建脚本,配置了整个项目在运行过程中会执行的脚本 
├── docs     		主题配置和md文档存放的位置
├── lib       	有关主题的配置工具类,以及webpack的配置等等
├── node_modules  外部库存放目录
├── test			
├── .eslintrc.js
├── .gitignore   	
├── package.json 	启动入口和依赖包安装指南
├── README.md  	说明文件
└── test
```

# 流程及VuePress源码解读

## 仓库地址
  
[VuePress的项目地址](https://github.com/vuejs/vuepress) 

## [config.js] 配置文件说明

### [config.js]所在的目录

![](http://ww1.sinaimg.cn/large/88b26e1cgy1fr7ewej00nj20fr0ij75u.jpg)

这个文件初看觉得很杂,什么样的配置都有,但是我们从本质出发,这个文件是为了做什么而定义的呢?

答案很简单,这个文件里面存放都是整个项目在构建过程中,你所定义的组件(或者自带的组件)要用到的相关配置信息,就像他的名字config.js一样,他只是一个配置文件.

那它是怎么用的呢? 举个例子:

比如我们在整个文件中,配置了一个[locales]对象:

![](http://ww1.sinaimg.cn/large/88b26e1cgy1fr7ewl6vgij20tk04mjrx.jpg)

那么它在项目中使用的方式就是:

![](http://ww1.sinaimg.cn/large/88b26e1cgy1fr7ews8avbj20vv0pr0z4.jpg)

在我们理解了这一点之后,整个文件变得很简单,我们不用关心这个字段的名字是什么,它所对应的又是什么,它只是我们在项目中会用到的一个变量的定义而已.

### [config.js] 中的坑

其实在[VuePress官网](https://vuepress.vuejs.org/zh/default-theme-config/) 已经有关于config.js里面你可以定制的项目,那这些配置的项目是如何生效的呢?

我们可以看一下`lib/dev.js`:

![](http://ww1.sinaimg.cn/large/88b26e1cgy1fr7ewxtussj209q0het9v.jpg)

可以看到这样一段代码:

![](http://ww1.sinaimg.cn/large/88b26e1cgy1fr7ex4mraoj20r804rdgl.jpg)

我们要明白这段代码的含义,要知道三点:

#### 我们传入的是什么?

要知道这里传入的`options.siteConfig.configureWebpack`这个值的含义,首先要知道option的来源:

在`dev.js`dev.js中它的定义是:

![](http://ww1.sinaimg.cn/large/88b26e1cgy1fr7exb4nebj214g02lgm5.jpg)

我们传入的资源路径`sourceDir`被`prepare`方法加工后得到了`option`,我们再看下`prepare `方法的定义:

位于`lib/prepare.js`路径下
