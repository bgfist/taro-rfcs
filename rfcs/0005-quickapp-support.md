- 提案时间: 2021-07-11
- 影响版本: 3.x
- 相关 Issues: 无

## 概述

增加对快应用平台的支持

## 动机

2.x已经支持快应用，3.x需要增加对快应用平台的支持。

## 详细设计

由于2.x已经支持快应用平台，所以主要任务在调研快应用如何支持3.x的架构，并将之前已有的实现代码进行迁移。

经调研，快应用没有动态模版，但是有动态组件component，所以可以用动态组件实现类似的模版功能。

仍然采用 *@tarojs/mini-runner* 打包

* 打包时处理快应用的模版/样式问题

* 需要提供*App/Page/Component*等小程序平台默认的全局函数
> 通过providerPlugin实现

* 提供*setData*实现
> 通过this.$set实现

* 提供与小程序一致的api接口
> 基于之前2.x的实现

* 把事件对象的类型 click 改为 tap，兼容小程序规范
> 修改 *@taro/react* 包相关判断

* 权衡之下，基于快应用原生语法去写这个抹平差异的组件库非常棘手，诸如属性不能透传，自定义事件参数与原生事件参数不一致的问题
> 决定提供原生组件，然后基于React/Vue各自用原生组件实现一套抹平差异后的组件库
* 快应用样式问题由开发者在业务代码中自行解决

## 缺陷

我们是不是可以不做这个功能，请考虑：

- 实现这个功能的投入：包括代码的复杂度、代码体积的增加、实现功能投入的人力
> 复杂度主要在抹平快应用api/组件库与小程序的差异，包括样式的处理，但是之前2.x已经做了许多工作
- 这个功能是不是不需要 Taro 提供，使用 Taro 的开发者也可以在应用层实现，甚至实现得更好
> 需要Taro提供
- 对 Taro 既有惯用开发习惯的影响
> 无
- 对已发布版本和现有功能的影响，以及用户进行迁移的成本
> 无
- 对其它未有代码实现的 RFC 提案的影响
> 无

## 替代选择

无


## 适配策略

* 样式需要适配快应用的样式
* 基础组件需要自己写一套抹平差异

无

PS: 已经实现了打包相关的逻辑，相关Pull Request: https://github.com/NervJS/taro/pull/9743
