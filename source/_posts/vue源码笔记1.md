---
title: vue源码笔记1
date: 2018-06-03 15:11:43
tags:
---
## 目的

1.加深理解 网页项目中vue 的机制  
2.学习相关的大型项目设计和构建的思路

## 项目结构
首先先看看项目结构，决定沿着什么逻辑层次去阅读源码，或者说是怎么去理解源码作者的思路。  
[结构原文地址](https://github.com/liutao/vue2.0-source/blob/master/Vue%E6%BA%90%E7%A0%81%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84%E6%95%B4%E7%90%86.md) 图片应该是2.5.X版本
    
    |—  build  打包相关的配置文件，其中最重要的是config.js。主要是根据不同入口，打包为不同的文件。  
    |—  dist 打包之后文件所在位置
    |—  examples 部分示例
    |—  flow 因为Vue使用了Flow来进行静态类型检查，这里定义了声明了一些静态类型
    |—  packages vue还可以分别生成其它的npm包
    |—  src 主要源码所在位置
        |— compiler 模板解析的相关文件（编译器）
            |— codegen 根据ast生成render函数
            |— directives 通用生成render函数之前需要处理的指令
            |— parser 模板解析
        |—  core 核心代码
            |— components 全局的组件，这里只有keep-alive
            |— global-api 全局方法，也就是添加在Vue对象上的方法，比如Vue.use,Vue.extend,,Vue.mixin等
            |— instance 实例相关内容，包括实例方法，生命周期，事件等
            |— observer 双向数据绑定相关文件
            |— util 工具方法
            |— vdom 虚拟dom相关
        |— entries 入口文件，也就是build文件夹下config.js中配置的入口文件。看源码可以从这里看起
        |— platforms 平台相关的内容
            |— web web端独有文件
            |— compiler 编译阶段需要处理的指令和模块
            |— runtime 运行阶段需要处理的组件、指令和模块
            |— server 服务端渲染相关
            |— util 工具库
        |— weex weex端独有文件
    |— server 服务端渲染相关
    |— sfc 暂时未知
    |—  shared 共享的工具方法
    |—  test 测试用例

![vue源码文件划分](/images/vue-code-file.png)  

所以，我们的目标应该是src文件夹下面除开weex和server外的内容

[参考文章](https://www.cnblogs.com/iovec/p/vue_01.html)