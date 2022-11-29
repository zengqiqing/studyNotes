vue3全家桶系列Pinia的使用小笔记

### 

### 背景：

单纯的做一些知识收集，vue3出来了，做点小跟风，去了解一下新的玩意儿。

### 目标：

pinia可以对Vue2和Vue3做到很好的支持，也就是老项目也可以使用Pinia。那么想尝试在vue项目中用上pinia。想试试跟vuex做一个对比，看看哪个用起来更爽！



初步认识：

- Pinia是Vue的状态管理库，允许跨组件/页面共享状态。

### 开始：

```JavaScript
1.npm init vite@latest //创建项目

2.选择模板---vue-ts

3.cd进项目安装好依赖包后，再单独安装pinia，执行 npm install pinia
```

![img](https://xingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=MGI4M2EyMjZmMmU5MWQ3YWZlNTkwZTg3NzFkYzM1MDZfUEI1WFZZZEU2NmV4UVNzN0hMRllDTnVhbmhIcFVjVTBfVG9rZW46Ym94Y25qVDM2NWNyMjYwNktVaFNYMnJzdXhmXzE2NDgzNDI3Mjc6MTY0ODM0NjMyN19WNA)

### 使用：

1. 在入口文件/src/main.ts中引入pinia
2. 在src文件夹中创建store文件夹，再建立一个`/src/store/index.ts `文件

```Perl
store文件夹中主要做的三件事是：

定义状态容器(仓库)

修改容器(仓库)中的 state

仓库中的 action 的使用
```

1. pinia初始代码片段：

```JavaScript
import { defineStore} from 'pinia'
/**

 * defineStore的使用：
 * defineStore的第一个参数是容器的名称，且名称不能重复。
 * defineStore的第二个参数是配置对象
 *  -state： 是用来存储全局的状态用的，这里边定义的，就能作SPA里全局的状态了
 *  -getters： 用来监视或者说是计算状态的变化的，有缓存的功能
 *  -actions：修改state全局状态数据的
 * **/ 
export const mainStore = defineStore('main',{
  state:()=>{
    return {
        count:1,//在state中定义的属性就是全局的状态数据，是每个页面和组件都可以通过Pinia方法读取到的
    }
  },
  getters:{},
  actions:{}
})
```



4.读取pinia：

pinia只需引入store后可以读取存储在store里的数据

对比：vuex读取数据则需要使用mapState来映射store中的数据，从而读取。

```JavaScript
//1.在业务文件中引入store
<template>
  <div class="">{{ store.count }}</div>
</template>
<script lang="ts" setup>
  import { mainStore } from "../store";
  const store = mainStore(); //创建store实例
</script>
```

5.状态数据结构和状态数据的修改

1. 新建/src/componment/test.vue文件
2. 使用pinia提供的`$patch`在test文件中修改store中的简单的count属性
3. 对比：vuex同步修改数据则需要通过commit方式来调用写在mutations里的方法来修改数据

```JavaScript
<template>
  <button @click="handleClick">点击改变count</button>
  <br/>
</template>

<script lang="ts" setup>
  import {mainStore} from '../store'
  const store = mainStore()
  // 点击事件
  const handleClick:() => void = ()=>{
      store.$patch({
      count:store.count+3,
    })
  }
</script>
```

2.1.往store中添加一个fastFood的属性，state中的fastFood属性可以监听到state中的count属性，从而可以进行计算

```javascript
<template>
  <div>
    <div>{{ store.count }}</div>
    <br />
    <div>{{ store.fastFood }}</div>
    <button @click="handleClick">点击改变count</button>
    <br />
  </div>
</template>

<script lang="ts" setup>
  import { mainStore } from "../store";
  const store = mainStore();
  // 点击事件
  const handleClick: () => void = () => {
    store.$patch((state) => {
    state.count = state.count + 3;
    state.fastFood = state.count % 2 === 0 ? "麦当劳" : "肯德基";
  });
};
</script>
```

2.2 使用pinia提供的aciton方法修改store中的属性

对比：vuex的Action 可以包含任意异步操作，使用action中的方法则需要使用dispatch来调用

```JavaScript
//pinia中使用action
// 文件：store/index.ts
import { defineStore } from "pinia";
export const mainStore = defineStore("main", {
  state: () => {
    return {
      count: 0,
      fastFood: "麦当劳", 
    };
  },
  getters: {},
  actions: {
    changState() {
      this.count++;
      this.fastFood = '猪脚饭'
    },
  },
});

//业务文件：/src/components/test
<template>
  <div>
    {{ count }}
  </div>
  <div>
    {{ fastFood }}
  </div>
  <button @click="changeAction">调用action方法</button>
</template>

<script lang="ts" setup>
  import { mainStore } from "../store";
  import { storeToRefs } from "pinia";
  const store = mainStore();
  const { count, fastFood } = storeToRefs(store);
  // 点击事件
  const changeAction = () => {
    store.changState();
  };
</script>
```



3.使用getters的缓存特性:

Pinia中的Getter和Vuex中的计算属性几乎一样，就是在获取State的值时作一些处理。注getters中是可以使用this来改变属性的！

场景：比如存储在state中有个电话号码，我们想输出的时候把中间四位展示为*号那么我们可以在getters中进行处理

```JavaScript
// 文件：store/index.ts
import { defineStore } from "pinia";
export const mainStore = defineStore("main", {
  state: () => {
    return {
      count: 0,
      fastFood: "麦当劳",
      phone: 15900000001,
    };
  },

  getters: {
    phoneHidden(state) {
      return state.phone
        .toString()
        .replace(/^(\d{3})\d{4}(\d{4})$/, "$1****$2");
    },
  },
});

//业务文件：/src/components/test
<template>
  <h2>一份{{ fastFood }}是{{ count }}元，电话热线:{{ phoneHidden }}</h2>
  <button @click="handleClick">换餐厅</button>
</template>

<script lang="ts" setup>
import { mainStore } from "../store";
import { storeToRefs } from "pinia";
const store = mainStore();
const { fastFood, count } = storeToRefs(store);
const { phoneHidden } = store;
const handleClick: () => void = () => {
  store.$patch((state) => {
    state.count = state.count + 3;
    state.fastFood = state.count % 2 === 0 ? "麦当劳" : "肯德基";
  });
};
</script>
```

### pinia多个store的使用

当出现多个store时，我们需要处理多个store中的数据互相调用的问题。

```JavaScript
// 1.新建多一个/store/goods.ts文件
import { defineStore } from "pinia";
export const goodStore = defineStore("good", {
  state: () => {
    return {
      list: [
        { id: 1, name: "麦当劳" },
        { id: 2, name: "真功夫" },
        { id: 3, name: "肯德基" },
      ],
    };
  },
});

//2.store/index.ts文件中引入good.ts文件
// 文件：store/index.ts
import { defineStore } from "pinia";
import { goodStore } from "./good";
export const mainStore = defineStore("main", {
  actions: {
    getGoodList() {
      console.log(goodStore().list, "----list ----");
    },
  },
});

//3.在componment/index中使用,最后可以在控制台中打印出good中的list属性值
<template>
  <button @click="changeAction">调用getGoodList方法</button>
</template>

<script lang="ts" setup>
import { mainStore } from "../store";
const store = mainStore();
const changeAction = () => {
  store.getGoodList();
};
</script>
```

### 踩坑：

在读取store的值时，觉得`<h1 class="">{{store.helloworld}}</h1>` 不优雅，想着解构成如下代码，但发现这样写当store里的数据发生变化时，template中的展示并没有变更过来。

```javascript
<template>
  <h1 class="">{{ helloworld }}</h1>
  <h2>{{ count }}</h2>
  <button @click="handleClick">点击改变count</button>
</template>

<script lang="ts" setup>
import { mainStore } from "../store";
const store = mainStore();
const { count } = store;

// 点击事件
const handleClick: () => void = () => {
  store.$patch((state: { count: number; fastFood: string; }) => {
    state.count = state.count + 3;
    state.fastFood = state.count % 2 === 0 ? "麦当劳" : "肯德基";
  });
};
</script>

```

![img](https://xingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=OWMzMDEwZWNhYTNmY2IxYjgzNDFjMmU0NTQ5MDcwNDdfZXUyQ0JlbFRFMnYyQng0N2RZc2t5eDZ6clpGWkJDSFFfVG9rZW46Ym94Y24wVEV2YmU5azdPdXJuOVpxR1lSU2loXzE2NDgzNDI3Mjc6MTY0ODM0NjMyN19WNA)



解决方案：

store是一个包裹的对象reactive，这意味着不需要.value在 getter 之后写入，但是，就像props在 中一样setup，我们无法对其进行解构，采用storeToRefs将数据变成响应式数据，这里的原理其实是把解构出来的数据作了ref响应式代理。所以数据拥有了响应式能力

```JavaScript
import {storeToRefs} from 'pinia';
const { helloworld,count } = storeToRefs(store);
```



### 体验：pinia应用到现有的旧的vue2.x项目中的体验

vuex的使用更严谨，每个store中的函数都有他的职责，同时页面的渲染需要更多的辅助函数（mapGetters，mapState....）辅助对store数据的读取和操作。虽然可以使用composition-api减少层次操作读取数据的层次，但pinia可以在这方面比vuex做得更出色

```javascript
//vuex代码

<template>
  <div>
    {{ vueMSG }}
    <input v-model="vueMSG" />
  </div>
</template>
<script>

import { computed } from '@vue/composition-api'
import store from '@/store/vuex'

export default {
  setup() {
    const vuexMSG = computed({
      get() {
        return store.state.message
      },
      set(val) {
        store.commit('update_MSG', val)
      },
    })
    return vuexMSG
  }
}
</script>

//pinia写法
<template>
  <div>
    <div>
      我是 {{ pinia.message }}
      <input v-model="pinia.message" />
    </div>
    <div>
      我是{{ piniaMSG }}
      <input v-model="piniaMSG" />
    </div>
  </div>
</template>
<script>

import { piniaStore } from '@/store/pinia'
import { computed } from '@vue/composition-api'

export default {
  name: 'Test',
  setup() {
    const pinia = piniaStore()
    const piniaMSG = computed({
      get() {
        return pinia.message
      },
      set(val) {
        pinia.message = val
      },
    })
    return {
      pinia,
      piniaMSG,
    }
  },
}

</script>
```

注意：

1.vue2.0还是要使用PiniaVuePlugin把pinia用起来，vue3.0不需要

2.$subscribe()可以用来侦听state的改变，和vuex的[subscribe](https://links.jianshu.com/go?to=https://vuex.vuejs.org/api/#subscribe)方法类似

#### 题外的补充

##### 在以上pinia的实例中有个`<script lang="ts" setup>` 代码，setup是干嘛用的？

在旧版本的vue中，setup的作用是导入并注册组件和导出一个字符串给template使用。如果模板上要使用的这些变量，必须要在 setup 返回的对象中定义。暴露变量必须 return 出来，如果我们内容很多的话，那么这个 setup 就会返回很多值。

```javascript
<template>
  <div>
    <Card>{{ data }}</Card>
  </div>
</template>

<script lang="ts">
import { defineComponent,ref } from "vue";
import Card from "./components/Card.vue";
export default defineComponent({
  components: {
    Card,
  },
  setup() {
    const data = ref("setup script");
    return { data };
  },
});
</script>

```

上面pinia示例代码中就有启用`setup script`，启用后省去了组件的注册步骤，也没有显式的导出变量的动作。你只需要在script上配置setup即可。

```javascript
<template>
  <div>
    <Card>{{ data }}</Card>
  </div>
</template>

<script lang="ts" setup>
import { ref } from "vue";
import Card from "./components/Card.vue";
const data = ref("setup script");
</script>

```

### 总结：

pinia与vuex的开发体验对比:

- Pinia的store 中 action 被调度变为常规的函数调用(store.xxx)，而不需使用 `dispatch` 方法或 `MapAction` 辅助函数，但辅助函数在vuex的使用来看还是很常见的。

- 使用Pinia的web应用程序会比使用Vuex更快。这种性能的提升可以归因于Pinia的轻量，Pinia体积约1KB。而vuex的体积约9.6KB。

- Pinia允许建立多个store，捆绑器代码将自动分割他们，Pinia 通过设计提供扁平结构,不支持嵌套存储，但仍然可以通过在另一个store中导入和使用store来隐式嵌套store，这样的组合更灵活，清晰度更高

- vuex拥有时间旅行能力，通过vuex的执行的操作会被记录下来并且可以选择操作记录，返回回退到某操作时的状态，而pinia暂不支持该功能

  

- #### 最后总结：

- Pinia 试图尽可能接近 Vuex 的理念。它更像是旨在测试 Vuex 下一次迭代的提案。

- pinia的简便和轻量更适用于复杂度不高且业务耦合性低的中小型项目中，而Vuex则更适用于大规模、高复杂度的 Vue项目