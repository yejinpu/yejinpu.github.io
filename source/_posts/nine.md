---
title: Vue1.0与2.0版本的区别
date: 2018-04-27 20:32:18
categories: vue
---
**高层级的变化**
&emsp;&emsp;1、模板解析器不再依赖于DOM（除非你使用真正的DOM作为模板），因此只要你使用字符串模板，你将不再受到任何1.0版本中的解析限制。但是，如果你依赖在存在的内容中挂载一个元素作为模板（使用el元素），你将依然受到这些限制。

&emsp;&emsp;2、编译器（将字符串模板转换为渲染方法的部分）和运行时间现在能够被分开。这里有两种不同的构建：  

* 独立构建：包括编译并且运行。这种方式和vue 1.0几乎完全一样。

* 运行时编译：由于它不包括编译器，在编译步骤时要么预编译模板，要么手动编写渲染功能。npm包默认导出这个版本，那么你需要有一个编译的过程（使用Browserify或Webpack ）,从中vueify或vue-loader将可以进行模板预编译。

**全局配置**

```  
    Vue.config.silent
    
    Vue.config.optionMergeStrategies
    
    Vue.config.devtools
    
    Vue.config.errorHandler（新API，全局的挂钩用于在组件渲染和监控的时候处理未捕获的错误）
    
    Vue.config.keyCodes（新API，为v-on配置自定义的key的别名）
    
    Vue.config.debug（已丢弃）
    
    Vue.config.async（已丢弃）
    
    Vue.config.delimiters(已丢弃)
    
    Vue.config.unsafeDelimiters（已丢弃，使用v-html）  
```

**全局API**
```  
    Vue.extend
    
    Vue.nextTick
    
    Vue.set
    
    Vue.delete
    
    Vue.directive
    
    Vue.component
    
    Vue.use
    
    Vue.mixin
    
    Vue.compile（新API，只能用于独立版本构建）
    
    Vue.transition
    
         - stagger（已丢弃，在el上设置
    
    Vue.filter
    
    Vue.elementDirective（已丢弃，使用组件）
    
    Vue.partial （已丢弃，使用功能组件）
```

**选项**  

data  
```
    data
    
         - props
          
         - prop
          
         - default
          
         - coerce（已丢弃，如果你需要转换prop,请使用compute属性）
          
         - prop binding modes（已丢弃，v-model在组件上可以工作
    
    propsData（新API）只能用于实例
    
    computed
    
    methods
    
    watch
```

DOM  
```
    el
    
    template
    
    render（新API）
    
    replace（已丢弃，组件现在必须有一个根元素）  
```

生命周期钩子  
```
    init（已丢弃，请使用beforeCreate）
    
    created
    
    beforeDestroy
    
    destroyed
    
    beforeMount(新API)
    
    mounted（新API）
    
    beforeUpdate（新API）
    
    updated（新API）
    
    activated(新API，用于keep-alive)
    
    deactivated（新API用于keep-alive）
    
    ready（已丢弃，使用mounted）
    
    activate（已丢弃，迁移到vue-router）
    
    beforeCompile（已丢弃，使用created）
    
    compiled（已丢弃，使用mounted）
    
    attached（已丢弃）
    
    detached（已丢弃，同上）  
```

Assets  
```
    directives
    
    components
    
    transitions
    
    filters
    
    partials（已丢弃）
    
    elementDirectives（已丢弃）   
```

杂项  
```
    parent
    
    mixins
    
    name
    
    extends
    
    delimiters（新API，替代原版的全局配置选项，只在独立构建中可用）
    
    functional（新API）
    
    events（已丢弃）  
```

**实例方法**  

data  
```
    vm.$watch
    
    vm.$get（已丢弃，直接检索值）
    
    vm.$set（已丢弃，使用Vue.set）
    
    vm.$delete（已丢弃，使用Vue.delete）
    
    vm.$eval（已丢弃，没有真正的使用）
    
    vm.$interpolate（已丢弃，同上）
    
    vm.$log（已丢弃，使用devtools）  
```

events  
```
    vm.$on
    
    vm.$once
    
    vm.$off
    
    vm.$emit
    
    vm.$dispatch（已丢弃，使用全局的事件或使用vuex，见下面）
    
    vm.$broadcast（已丢弃，同上）  
```

DOM  
```
    vm.$nextTick
    
    vm.$appendTo（已丢弃，在 vm.$el上使用本地API）
    
    vm.$before（已丢弃）
    
    vm.$after（已丢弃）
    
    vm.$remove（已丢弃）  
```

生命周期  
```
    vm.$mount
    
    vm.$destroy  
```

**指令**  
```
    v-text
    
    v-html（注意{{{ }}} 被丢弃）
    
    v-if
    
    v-show
    
    v-else
    
    v-for
    
        - key (替代 track-by)
      
        - object v-for
      
        - range v-for
      
        - 参数顺序更新：数组中使用(value, index) in arr，对象中使用(value, key, index) in obj
      
        - $index和$key被丢弃
    
    v-on
    
        - modifiers
        
        - on child component
        
        - 自定义键码，目前版本Vue.config.keyCodes代替原来的Vue.directive('on').keyCodes
    
    v-bind
    
        - 作为prop
        
        - xlink
        
        - 绑定对象
    
    v-bind:style
    
        - prefix sniffing
    
    v-bind:class
    
    v-model
    
        - lazy (as modifier)
      
        - number (as modifier)
      
        - ignoring composition events
      
        - debounce（已丢弃，使用v-on:input）
    
    v-cloak
    
    v-pre
    
    v-once（新API）
    
    v-ref（已丢弃，现在只是一个特殊的属性ref）
    
    v-el（和ref合并）  
```

**特殊组件**  
```
    <component>
    
        - :is
        
        - async组件
        
        - inline-template
    
    <transition>
    
    <transition-group>
    
    <keep-alive>
    
    <slot>
    
    partial（已丢弃）  
```

**特殊属性**  
```
    key
    
    ref
    
    slot  
```

**服务器端渲染**  
```
    renderToString
    
    renderToStream
    
    client-side hydration  
```