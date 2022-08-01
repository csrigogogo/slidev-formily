---
title: 表单
---

# 表单

Antd Form 与 formily 的 开发比较 以及 原理分析

<div class="absolute bottom-10">
  <span class="font-700">
    主讲人: Logan
  </span>
</div>

<!--
- rc-field-form 的源码分析
- formily的使用 对比antd form 

（一分钟） 
本次分享主要聚焦在antd form 的源码分析。以及formily 的使用和antd form 的不同， 

formily解决了 什么样的问题

最后会使用一个贴近业务的例子来说明
-->

---

<div class="grid grid-cols-2 gap-4">
<div>

# 什么是表单?
收集数据并提交

- 收集数据
- 校验数据
- 提交数据

</div>

<div>

# HTML \<form>

[form -- mdn](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/form)

- 提交数据 XMLHttpRequest

- actions

![origin_form](/origin_form.png)



</div>

</div>

<!--
（5分钟）重点介绍一下

- 收集数据
form 标签 会收集
name(html 5) 或者 id(html 4) 独一无二 的原始表单控件

- 校验数据
可以看到表单控件内置了了一些校验  
例如type: email 

- 提交数据
原始form标签 获取提交数据的方式
原始form 表单的提交方式其实对应的也是 antd Form提交数据的方式
onSubmit   和 actions

缺点： 逻辑和模版耦合
通常来说我们要灵活一点控制数据 只能通过获取dom节点然后获取数据，进而控制数据。
原始方法维护性差 耦合
-->

---
layout: center
---

# 代码可维护性

<!--
html form 的代码 很难维护  
 
如何做到代码可维护
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

- model:  业务建模, 偏向于处理业务数据的处理逻辑
- view: 负责维护 UI 结构和样式
- viewModel: 视图模型 和 view进行数据双向绑定, 偏向于处理交互层的逻辑

<br/>

## 目的

- 逻辑, 视图拆分
- 数据驱动



</div>
</div>

<!--
（ 5 分钟 ）
mvvm 软件工程的概念

如何定义代码可维护
- 代码是模块化的，逻辑是分层的，
- Service是一层，View是一层，核心业务逻辑是一层，事件处理逻辑是需要与jsx层做严格隔离的。


vue 的响应式模型  在这样的思想驱动下 诞生的
 
我们得以可以操纵数据来控制视图渲染， 而不需要直接操作dom

这里跟react 的受控表单形式有差别

- 受控组件并不维护自己的内部状态，外部提供 props 和回调函数，受控组件通过 props 被控制起来，更新通过 onChange 通知给父组件。

- 非受控组件，则把表单数据将交由 DOM 节点来处理，通过 Ref 来操作 DOM 获取表单的值再加以操作
-->

---
layout: center
---

# Antd Form

<!--
（2 分钟 ）
第一个主角 是我们常使用的组件

antd form如果有同学关注过源代码的话 会发现antdform

只是对 rc-field-form 的封装，antd 基本只做了视图上面的变化，想看form的关键逻辑 我们得去rc-field-form，所以我们直奔主题

https://github.com/react-component/field-form
-->

---

# rc-field-Form

构建在 Context 基础上的 受控表单方案 

- FormStore : 整个表单的核心类， 管理表单数据，生成表单实例， 控制表单校验
- Form 组件 : 
- Field 组件(Form.Item): 向上向FormStore注册自身， 向下传递受控属性 将表单组件变为受控组件
- FormInstance: FormStore.getForm() 返回的结果

<!--
## 为什么要阅读源代码

- 更好的使用api, 了解这些 api 其背后的逻辑以及封装思想 
- 学习开源项目中的编程思想,提升自己的代码设计能力

（5 分钟）

简单介绍Context

const ThemeContext = React.CreateContext(null)

ThemeContext.Provider

ThemeContext.Consumer

受控组件

受控组件并不维护自己的内部状态，外部提供 props 和回调函数，受控组件通过 props 被控制起来，更新通过 onChange 通知给父组件。


- FormStore 
  是一个类，存储了 Form 表单数据，并定义了各种对数据的操作- 属于整个组件的核心

- Form组件
 

- Field组件
-->

---

# FormStore

```ts
const [form] = Form.useForm();

{
    getFieldValue: this.getFieldValue,
    getFieldsValue: this.getFieldsValue,
    getFieldError: this.getFieldError,
    getFieldWarning: this.getFieldWarning,
    getFieldsError: this.getFieldsError,
    isFieldsTouched: this.isFieldsTouched,
    isFieldTouched: this.isFieldTouched,
    isFieldValidating: this.isFieldValidating,
    isFieldsValidating: this.isFieldsValidating,
    resetFields: this.resetFields,
    setFields: this.setFields,
    setFieldValue: this.setFieldValue,
    setFieldsValue: this.setFieldsValue,
    validateFields: this.validateFields,
    submit: this.submit,
    _init: true,
}
```


<!--
这里的 form 实例  实际是 formStore.getForm() 返回的结果

form 实例上的方法 都是 FormStore 已经实现了的

-->

---

# Form

```tsx {1,17|all}
<Component
      {...restProps}
      onSubmit={(event: React.FormEvent<HTMLFormElement>) => {
        event.preventDefault();
        event.stopPropagation();

        formInstance.submit();
      }}
      onReset={(event: React.FormEvent<HTMLFormElement>) => {
        event.preventDefault();

        formInstance.resetFields();
        restProps.onReset?.(event);
      }}
    >
      {wrapperNode}
</Component>
```

<!--
1. 这里的Component 组件是动态的有点类似Vue里的动态组件
React中比较灵活
它的默认值是 ‘form’你也可以传递props 来自定义这个组件但可能你要实现相应的api， 不在讨论范围
这里的我们平常获取表单数据 习惯了form.getFieldsValue()
并不会触发onSubmit。 除非你的提交按钮设置了htmlType=“submit”


2. wrapperNode自然是Form组件里面的内容 但在外面套了一层FieldContext， 用于派发formContext（其实就是form实例）， 将form实例下发到具体的Field实现 在子组件内部控制formInstance的内容变化
```tsx
<FieldContext.Provider value={formContextValue}>{childrenNode}</FieldContext.Provider>import { Field } from 'rc-field-form'

const formContextValue = React.useMemo(
    () => ({
      ...(formInstance as InternalFormInstance),
      validateTrigger,
    }),
    [formInstance, validateTrigger],
);
<FieldContext.Provider value={formContextValue}>
	 <Field name="username">
    <Input placeholder="Username" />
  </Field>
  <Field name="password">
    <Input placeholder="Password" />
  </Field>
</FieldContext.Provider>
```
-->

---

# Field
```tsx
class Field {
   constructor(props: InternalFieldProps) {
    super(props);
    // wrapperField 已经将外层的context 消费 并通过props 传递进来
    if (props.fieldContext) {
      const { getInternalHooks }: InternalFormInstance = props.fieldContext;
      const { initEntityValue } = getInternalHooks(HOOK_MARK);
      initEntityValue(this);
    }
  }...
}
const WrapperField = ({ name, ...restProps }) => {
  const fieldContext = React.useContext(FieldContext); // 消费了刚才的context
  const namePath = name !== undefined ? getNamePath(name) : undefined;
  let key: string = 'keep';
  if (!restProps.isListField) {
    key = `_${(namePath || []).join('_')}`;
  }
  ...
  return <Field key={key} name={namePath} {...restProps} fieldContext={fieldContext} />;
}
export default WrapperField;
```

<!--
插一下提问 
为啥要消费fieldContext在通过props传进去
因为Class组件的 constructor 不能使用Context api

向上
Field组件 首先消费了外界的fieldContext(formInstance)从而拥有了和FormStore通信的能力

向下

Field组件 包裹着我们的UI组件 类似于Input之类的

所以field组件 还起着连接的作用，将我们的UI组件 变成受控组件并且和 Store 数据联系在一起
-->

---
layout: center
---

# Formily

<!--
简单介绍一下formily

阿里巴巴推出的统一前端表单解决方案

针对于表单的领域问题给出了他的答案

我们内部的基础组件 Schema-page 便是基于此开发

或多或少接触到了 schema 开发页面

关于 formily 解决的领域问题太多, 官方文档有很详尽的介绍,
- 精确渲染
- 领域模型 
- 路径系统
- 生命周期
- 协议驱动  
- 分层架构

这里主要介绍一下 生命周期 , 以及协议驱动的领域问题
-->

---

# formily

为什么要使用formily 


- 动态化渲染表单的能力， 以及通过协议描述表单的能力

- 逻辑可跨框架，跨终端复用 使用桥接层 @formily/react  @formily/vue 等来确保核心逻辑可跨越框架迁移
- UI 核心 分离， 使用独立视图层 @formily/antd @formily/element 。。。 等等

<img src="/formily.png" class="m-10 h-60 center  shadow" />

<!--
(三分钟) 

- 协议驱动

或多或少 因为antd form 的结构产生 想要优化的念头
    antd form 没有原生就支持这样的动态化渲染的能力
    但通常只能在上层在封装一层动态化渲染机制，这样就得基于某个JSON 协议来驱动渲染（可能是自己定义的  可能是标准协议）

- dsaas

分层架构
-->

---

# formily 架构模式

放弃受控模式，改用响应式受控的形式

<img class="m-2 w-auto h-100 shadow" src="https://img.alicdn.com/imgextra/i3/O1CN01iEwHrP1NUw84xTded_!!6000000001574-55-tps-1939-1199.svg" alt="">

<!--
formily本身属于 monoRepo 的开发模式,即所有的包位于一个仓库

涉及到formily的包还是挺多,最关键的几个包
- @formily/core
   建立了相关模型, 属于 viewmodel 层
- @formily/react
   桥接层  属于view层 专门用来与@formily/core 做桥接通讯的
- @fomily/antd
- @formily/reactive (响应式开发 类似 mobx) 不展开讲 

那 model 层在哪  当然是我们自己的业务代码了, 至于我们自己是用 OOP 的方式 还是 函数式 FP 的方式维护 model 层
这个就不是 formily 需要关注的事情了

当然不止这些包还包括一些例如
@formily/path
@formily/validator
@formily/json-schema 等工具包它们可以说每一个都解决了各自的领域问题,共同组成了 formily
-->

---
layout: center
---

# Formily 实际操作一波





<!-- (值联动的精确渲染)[https://github.com/alibaba/formily/discussions/2432] -->

<!--
真实例子地址：
https://cps-api.sk8s.cn/finance/receivable/rebate-invoice

打开本地lowcode-component-v2 项目
启动然后展示写好的例子
-->

---


# 未来：formily Designer

https://designable-antd.formilyjs.org/

![formilyDesigner](/formilydesigner.png)

<!--
## codesandbox

https://codesandbox.io/s/practical-tdd-lcecmp?file=/App.tsx

步骤
使用 formily designer来设计表单 同步替换到 codesandbox
展示一下效果
-->

---
layout: center
---

# 最后说几句

<!--
formily 只是一个表单解决方案 ， 它重新考虑了很多如今的表单方案上存在的问题， 并给出了自己的答案， 但不免也会引入许多概念使我们学习它的成本很高， 至于学习它值不值， 每个心中可能有不同的答案， 他能解决你的痛点，能节约你的时间就很值
如果你对其他的表单方案不满意，想要改变， 不妨试一试 也许会有不一样的开发体验
-->
