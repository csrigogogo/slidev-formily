# 表单

Antd Form 与 formily的 开发比较 以及 原理分析

<div class="absolute bottom-10">
  <span class="font-700">
    主讲人: Logan
  </span>
</div>

<!-- ## 
    针对痛点进行讲解  
    
-->

---


<div class="grid grid-cols-2 gap-4">
<div>

# 什么是表单?
收集数据并提交

- 收集数据
- 校验数据
- 表单布局
- 数据路径
</div>

<div>

# HTML form


</div>

</div>






---
layout: center
--- 
 # 代码可维护性

<!-- 

代码是模块化的，逻辑是分层的，Service是一层，View是一层，核心业务逻辑是一层，事件处理逻辑是需要与jsx层做严格隔离的。

 -->


---


<div class="grid grid-cols-2 gap-4">
<div>

# MVVM
Model View ViewModel

![MVVM](/mvvm.svg)

</div>
<div>

# 介绍

- model:  业务建模, 处理业务逻辑
- view: 处理视图逻辑
- viewModel: 

<br/>

## 目的

- 逻辑, 视图拆分
- 数据驱动



</div>
</div>

---

# Antd Form

构建在 Context 基础上的 受控表单方案 



---

# Field



---
layout: center
---
# Formily

---

# formily

为什么要使用formily 


- 动态化渲染表单的能力， 以及通过协议描述表单的能力

- 逻辑可跨框架，跨终端复用 ，UI 核心 分离， 使用独立视图层 @formily/antd @formily/element 。。。 等等

<img src="/formily.png" class="m-10 h-60 center  shadow" />


<!-- 
    或多或少 因为antd form 的结构产生 想要优化的念头
    antd form 没有原生就支持这样的动态化渲染的能力
    但通常只能在上层在封装一层动态化渲染机制，这样就得基于某个JSON 协议来驱动渲染（可能是自己定义的  可能是标准协议）
 -->


---

# formily 架构模式

放弃受控模式，改用响应式受控的形式

<img class="m-2 w-auto h-100 shadow" src="https://img.alicdn.com/imgextra/i3/O1CN01iEwHrP1NUw84xTded_!!6000000001574-55-tps-1939-1199.svg" alt="">

---

# formily 跨端开发效果演示




---
layout: center
---
# Formily 实际操作一波





(值联动的精确渲染)[https://github.com/alibaba/formily/discussions/2432]


---
layout: center
---

# 最后说几句

<!--
formily 只是一个表单解决方案 ， 它重新考虑了很多如今的表单方案上存在的问题， 并给出了自己的答案， 但不免也会引入许多概念使我们学习它的成本很高
 
    如果你对其他的表单方案不满意，想要改变， 不妨试一试 也许会有不一样的开发体验
-->
