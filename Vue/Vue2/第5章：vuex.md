是什么？
用于在vue中实现集中式状态（数据）管理的一个vue插件，对vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式（任意组件通信）。


什么时候用？
1. ==多个组件==依赖同一状态（之前是不是最多两个组件间通信，且都是单向的，一个发，一个收）
2. 来自不同组件的行为需要变更（获取）同一状态。

![[Pasted image 20250114155939.png]]
![[Pasted image 20250114160112.png]]


原理图
![[Pasted image 20250114164418.png]]

# 5.1 使用步骤
vue2中，需要使用vuex的3版本
vue3中，需要使用vuex的4版本

## 1. 安装vuex
	npm i vuex@3  （直接npm i vuex安装的最新版）


## 2. 创建store
   src/store/index.js
   ```js
import Vuex from 'vuex'
import Vue from 'vue'
Vue.use(Vuex) //先安装vuex，才能创建store

const actions = { // 用于处理异步操作
    increment(context, value) {
        // 此处省略调用后台接口的逻辑
        // ...
      context.commit('increment',value)
    },
    // decrement(context,value) { 
    //   context.commit('decrement',value)
    // }
}

const  mutations = { // 用于操作数据
    increment(state,value) {
      state.count+=value
    },
    decrement(state,value) {
      state.count-=value
    }
}

const state = { // 用于存储数据
  count: 0
}

//创建store
const store = new Vuex.Store({
  actions,
  mutations,
  state
})

// 导出store
export default store
```

## 3. 配置store
   main.js
   ```js
import Vue from 'vue'
import App from './App.vue'
import store from './store'

Vue.config.productionTip = false
const vm = new Vue({
  render: h => h(App),
  store:store,
}).$mount('#app')

```


## 4. 使用vuex
App.vue
```vue
<template>
  <div id="app" class="container">
    <Count />
  </div>
</template>

<script>
import Count from "./components/Count.vue";

export default {
  name: "App",
  components: { Count },
};
</script>

<style>
.container {
  display: flex;
  justify-content: space-around;
}
</style>

```


Count.vue

本案例中只有一个Count组件去修改共享数据，没有体现出共享，但是再多建几个组件也是同样的道理。
```vue
<template>
  <div>
    <h3>当前Count值为：{{ $store.state.count }}</h3>
    n的值为<input type="text" v-model.number="n" />
    <button @click="increment">+n</button>
    <button @click="decrement">-n</button>
  </div>
</template>

<script>
export default {
  name: "Count",
  data() {
    return {
      n: 1,
    };
  },
  methods: {
    increment() {
      // 异步调用
      this.$store.dispatch("increment", this.n);
    },
    decrement() {
	  // 直接修改
      this.$store.commit("decrement", this.n);
    },
  },
};
</script>

<style></style>

```
![[Pasted image 20250114174454.png]]

# 5.2 getters(vuex中的计算属性)

src/store/index.js
```js
import Vuex from 'vuex'
import Vue from 'vue'
Vue.use(Vuex) //先安装vuex，才能创建store

const actions = { // 用于处理异步操作
    increment(context, value) {
        // 此处省略调用后台接口的逻辑
        // ...
      context.commit('increment',value)
    },
    // decrement(context,value) { 
    //   context.commit('decrement',value)
    // }
}

const  mutations = { // 用于操作数据
    increment(state,value) {
      state.count+=value
    },
    decrement(state,value) {
      state.count-=value
    }
}

const state = { // 用于存储数据
  count: 0
}
//getters，相当于计算属性
const getters = {
    BigCount(state) {
      return state.count*10
    }
}

//创建store
const store = new Vuex.Store({
  actions,
  mutations,
    state,
  getters
})

// 导出store
export default store
```

Count.vue
```vue
<template>
  <div>
    <h3>当前Count值为：{{ $store.state.count }}</h3>
    <h3>当前Count放大10倍的值为：{{ $store.getters.BigCount }}</h3>
    n的值为<input type="text" v-model.number="n" />
    <button @click="increment">+n</button>
    <button @click="decrement">-n</button>
  </div>
</template>

<script>
export default {
  name: "Count",
  data() {
    return {
      n: 1,
    };
  },
  methods: {
    increment() {
      // 异步调用
      this.$store.dispatch("increment", this.n);
    },
    decrement() {
      this.$store.commit("decrement", this.n);
    },
  },
};
</script>

<style></style>

```

# 5.3 Vuex中4个map方法
![[Pasted image 20250114194426.png]]
![[Pasted image 20250114194456.png]]
Count.vue
```vue
<template>
  <div>
    <h3>当前Count值为：{{ count }}</h3>
    <h3>当前Count放大10倍的值为：{{ BigCount }}</h3>
    n的值为<input type="text" v-model.number="n" />
    <button @click="increment(n)">+n</button>
    <button @click="decrement(n)">-n</button>
  </div>
</template>

<script>
import { mapState, mapGetters, mapMutations, mapActions } from "vuex";
export default {
  name: "Count",
  data() {
    return {
      n: 1,
    };
  },
  computed: {
    // count() {
    //   return this.$store.state.count;
    // },
    ...mapState(["count"]),
    // ...mapState({'count':'count'}),
    // BigCount() {
    //     return this.$store.getters.BigCount;
    // },
    ...mapGetters(["BigCount"]),
    // ...mapGetters({'BigCount':'BigCount'}),
  },
  methods: {
    // increment() {
    //   // 异步调用
    //   this.$store.dispatch("increment", this.n);
    // },
    ...mapActions(["increment"]),
    // ...mapActions({'increment':'increment'}),
    // decrement() {
    //   this.$store.commit("decrement", this.n);
    // },
    ...mapMutations(["decrement"]),
    // ...mapMutations({'decrement':'decrement'}),
  },
};
</script>

<style></style>


```


# 5.4 vuex模块化+namespace
重要，大型项目中分模块，我先不学，太多了，用到了再说
toLearn....


