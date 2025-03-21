# 1. 认识Vue3

**vue3的优势：**
更容易维护（组合式API）
更快的速度 （重写diff算法，模版编译优化，更高效的组件初始化）
更小的体积
更优的数据响应式（Proxy代替defineProperty() ）

 

![[Pasted image 20250118134836.png]]

# 2. create-vue 搭建Vue3项目
**认识create-vue**
![[Pasted image 20250118135245.png]]

**使用vue-create创建项目**
1. 前提环境条件
   16.0以上版本的node.js
2. 执行npm init vue@latest 
   这一指令会安装并执行create-vue


# 3. 熟悉项目目录和关键文件
![[Pasted image 20250118140549.png]]

# 4. 组合式API - setup选项
![[Pasted image 20250118142023.png]]
==由于setup创建的时机太早，在该函数中无法获取this==（在vue3中，不用纠结this指向谁了）


![[Pasted image 20250118142322.png]]
如果要使用setup中的数据或者函数，需要return出去。
![[Pasted image 20250118142505.png]]

# 5. 组合式API- reactive和ref函数
在vue3中，数据默认不是响应式的
![[Pasted image 20250118142855.png]]
![[Pasted image 20250118143344.png]]

# 6. 组合式API-computed
![[Pasted image 20250118143804.png]]
# 7. 组合式API-watch
![[Pasted image 20250118144649.png]]
![[Pasted image 20250118144752.png]]
![[Pasted image 20250118144844.png]]

# 8. 组合式API-生命周期函数
![[Pasted image 20250118145719.png]]

# 9. 组合式API-父子通信
由于使用了组合式API的写法（setup语法糖，就没办法写setup函数同级的props配置项了，但是可以通过defineProps“编译器宏）
![[Pasted image 20250118150223.png]]
App.vue
```vue
<script setup>
import { ref } from "vue";
import SonCom from "./components/SonCom.vue";

const msg = "Hello World!"; //非响应式数据
const money = ref(100); //响应式数据(基本类型)
const car = ref({
  name: "BMW",
  price: 500000,
}); //响应式数据(对象类型)
</script>

<template>
  <div class="parentDiv">
    我是父组件
    <SonCom :msg="msg" :money="money" :car="car" />
  </div>
</template>

<style>
.parentDiv {
  background-color: antiquewhite;
  border: 1px solid black;
}
</style>

```

SonCom.vue
```vue
<script setup>
const props = defineProps({
  msg: String,
  money: Number,
  car: Object,
});
console.log(props.msg, props.money, props.car);
</script>

<template>
  <div class="SonDiv">
    我是子组件， 父组件传过来的值：
    <div>消息是：{{ msg }}</div>
    <div>钱是：{{ money }}</div>
    <div>车是：{{ car }}</div>
  </div>
</template>

<style>
.SonDiv {
  background-color: aqua;
  border: 1px solid red;
  margin: 10px;
}
</style>

```


![[Pasted image 20250118151003.png]]

App.vue
```vue
<script setup>
import { ref } from "vue";
import SonCom from "./components/SonCom.vue";

const money = ref(100); //响应式数据(基本类型)
const earnMoney = () => {
  money.value += 100;
};
const spendMoney = (cost) => {
  money.value -= cost;
};
</script>

<template>
  <div class="parentDiv">
    我是父组件
    <div>当前还有多少钱：{{ money }}</div>
    <button @click="earnMoney">挣钱</button>
    <!-- 通过组件自定义事件，实现子---》父 -->
    <SonCom @spendMoneyEvent="spendMoney" />
  </div>
</template>

<style>
.parentDiv {
  background-color: antiquewhite;
  border: 1px solid black;
}
</style>

```

SonCom.vue
```vue
<script setup>
const emit = defineEmits(["spendMoneyEvent"]);
const spendMoney = () => {
  emit("spendMoneyEvent", 100);
};
</script>

<template>
  <div class="SonDiv">
    我是子组件， 我要修改父组件的数据：
    <button @click="spendMoney">花钱</button>
  </div>
</template>

<style>
.SonDiv {
  background-color: aqua;
  border: 1px solid red;
  margin: 10px;
}
</style>

```


# 10. 组合式API-模版引用
![[Pasted image 20250118153757.png]]
![[Pasted image 20250118153837.png]]



![[Pasted image 20250118154338.png]]


>[!note] 模版引用的时机
>组件挂载完毕
>

App.vue
```vue
<script setup>
import { ref } from "vue";
import RefCom from "@/components/RefCom.vue";

const inputRef = ref(null);
const comRef = ref(null);
const opearateCom = () => {
  console.log(comRef.value);
  // 操作子组件的数据
  comRef.value.count++;
  // 调用子组件的方法
  comRef.value.myFn();
};
</script>

<template>
  <div class="parentDiv">
    <input type="text" ref="inputRef" /><button @click="inputRef.focus()">
      让input聚焦
    </button>
    <hr />
    <RefCom ref="comRef" /> <button @click="opearateCom">操作组件</button>
  </div>
</template>

<style>
.parentDiv {
  background-color: antiquewhite;
  border: 1px solid black;
}
</style>

```


RefCom.vue
```vue
<script setup>
import { ref } from "vue";

const count = ref(0);
const myFn = () => {
  console.log("我是子组件的函数");
};
defineExpose({ count, myFn });
</script>

<template>
  <div class="SonDiv">子组件的数据{{ count }}</div>
</template>

<style>
.SonDiv {
  background-color: aqua;
  border: 1px solid red;
  margin: 10px;
}
</style>

```

# 11. 组合式API-provide 和 inject
爷爷和孙子之间互相传递
![[Pasted image 20250118160418.png]]
![[Pasted image 20250118160504.png]]
![[Pasted image 20250118161149.png]]
![[Pasted image 20250118161158.png]]

App.vue
```vue
<script setup>
import SonCom from "@/components/SonCom.vue";
import { provide, ref } from "vue";

const msg = "祖训：温故而知新，可以为师也";
const money = ref(1000000);
provide("msg-key", msg);
provide("money-key", money);

const spendMoney = (cost) => {
  money.value -= cost;
};
provide("spendMoney-key", spendMoney);
</script>

<template>
  <div class="appDiv">
    <h3>我是祖先（爷爷）</h3>
    <SonCom />
  </div>
</template>

<style>
.appDiv {
  background-color: antiquewhite;
  border: 1px solid black;
}
</style>

```

SonCom.vue
```vue
<script setup>
import GrandSonCom from "./GrandSonCom.vue";
</script>

<template>
  <div class="SonDiv">
    <h4>我是儿子</h4>
    <GrandSonCom></GrandSonCom>
  </div>
</template>

<style>
.SonDiv {
  background-color: aqua;
  border: 1px solid red;
  margin: 10px;
}
</style>

```

GrandSonCom.vue
```vue
<script setup>
import { inject } from "vue";

const msg = inject("msg-key");
const money = inject("money-key");
const spendMoney = inject("spendMoney-key");
</script>

<template>
  <div class="GrandSonDiv">
    <h5>我是孙子</h5>
    <div>我们家的祖训：{{ msg }}</div>
    <div>
      传下来的钱：{{ money }} <button @click="spendMoney(5)">花钱</button>
    </div>
  </div>
</template>

<style>
.GrandSonDiv {
  background-color: beige;
  border: 1px solid red;
  margin: 10px;
}
</style>

```


# 12. Vue3.3新特性-defineOptions
![[Pasted image 20250118163607.png]]
![[Pasted image 20250118163641.png]]

# 13. Vue3.3新特性-defineModel
![[Pasted image 20250118164019.png]]

## 13.1 没有该新特性，封装组件
App.vue
```vue
<script setup>
import MyInput from "@/components/MyInput.vue";
import { ref } from "vue";

const inputValue = ref("");
</script>

<template>
  <div class="appDiv">
    <MyInput v-model="inputValue" />
    <p>{{ inputValue }}</p>
  </div>
</template>

<style>
.appDiv {
  background-color: antiquewhite;
  border: 1px solid black;
}
</style>

```

MyInput.vue
```vue
<script setup>
const props = defineProps({
  modelValue: String,
});
const emit = defineEmits(["update:modelValue"]);
</script>

<template>
  <div class="SonDiv">
    <input
      type="text"
      :value="modelValue"
      @input="(e) => emit('update:modelValue', e.target.value)"
    />
  </div>
</template>

<style>
.SonDiv {
  background-color: aqua;
  border: 1px solid red;
  margin: 10px;
}
</style>

```

## 13.2 引入以后
MyInput.vue
```vue
<script setup>
import { defineModel } from "vue";
const modelValue = defineModel();
</script>

<template>
  <div class="SonDiv">
    <input
      type="text"
      :value="modelValue"
      @input="(e) => (modelValue = e.target.value)"
    />
  </div>
</template>

<style>
.SonDiv {
  background-color: aqua;
  border: 1px solid red;
  margin: 10px;
}
</style>

```

# 14. Pinia 快速入门
![[Pasted image 20250118193112.png]]

## 14.1 添加Pinia到项目中
初次学习，我们自己加，后面可以借助create-vue脚手架直接安装。
1. 使用Vite创建一个Vue3项目
   npm create vue@latest
2. 按照官网文档 安装pinia到项目中
	npm install pinia
main.js
```js
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'


const pinia = createPinia()
const app = createApp(App)

app.use(pinia)
app.mount('#app')
```

3. 定义store
src/store/counter.js
```js
import { defineStore } from 'pinia'
import { computed, ref } from 'vue'

export const useCounterStore = defineStore('counter', () => {

    const count = ref(0)
    const doubleCount = computed(() => count.value * 2)
    function increment() {
      count.value++
    }
    function decrement() {
        count.value--
     }
  
    return { count, doubleCount, increment,decrement }
})
```

4. 编写组件使用store
	App.vue
```vue
<script setup>
import Son1Com from "@/components/Son1Com.vue";
import Son2Com from "@/components/Son2Com.vue";
import { useCounterStore } from "@/store/counter.js";

const counterStore = useCounterStore();
</script>

<template>
  <div>
    <h3>
      我是根组件, 当前count是{{ counterStore.count }}, doubleCount是{{
        counterStore.doubleCount
      }}
    </h3>
    <Son1Com />
    <Son2Com />
  </div>
</template>

<style scoped></style>

```
Son1Com.vue
```vue
<script setup>
import { useCounterStore } from "@/store/counter.js";

const counterStore = useCounterStore();
</script>

<template>
  <div>
    <h3>我是Son1组件, 共享数据count{{ counterStore.count }}</h3>
    <button @click="counterStore.increment">+</button>
  </div>
</template>

<style scoped></style>

```
Son2Com.vue
```vue
<script setup>
import { useCounterStore } from "@/store/counter.js";

const counterStore = useCounterStore();
</script>

<template>
  <div>
    <h3>我是Son2组件, 共享数据count{{ counterStore.count }}</h3>
    <button @click="counterStore.decrement">-</button>
  </div>
</template>

<style scoped></style>

```

## 14.2 action异步实现

![[Pasted image 20250118202708.png]]
npm install axios

channel.js
```js
import axios from 'axios'
import { defineStore } from 'pinia'
import { ref} from 'vue'

export const useChannelStore = defineStore('channel', () => {

    const channelList = ref([])
    const fetchData = async ()=> { 
        const { data: { data } } = await axios.get('http://geek.itheima.net/v1_0/channels')
        channelList.value = data.channels
        
    }
    return {channelList,fetchData}
  
})
```
App.vue
```vue
<script setup>
import Son1Com from "@/components/Son1Com.vue";
import Son2Com from "@/components/Son2Com.vue";
import { useCounterStore } from "@/store/counter.js";
import { useChannelStore } from "@/store/channel.js";

const counterStore = useCounterStore();
const channelStore = useChannelStore();
</script>

<template>
  <div>
    <h3>
      我是根组件, 当前count是{{ counterStore.count }}, doubleCount是{{
        counterStore.doubleCount
      }}
    </h3>
    <Son1Com />
    <Son2Com />

    <hr />
    <button @click="channelStore.fetchData">获取channel列表数据</button>
    <div>
      <ul>
        <li v-for="item in channelStore.channelList" :key="item.id">
          {{ item.name }}
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped></style>

```

## 14.3 storeToRefs方法

为什么解构为丢失响应式？
因为vue3中是ref和reactive方法包装数据，都是生成一个对象，响应式是基于对象实现的，
如果对象不在了，里面的属性自然丢失响应式。

```vue
<script setup>
import { useCounterStore } from "@/store/counter.js";
import { useChannelStore } from "@/store/channel.js";
import { storeToRefs } from "pinia";

const counterStore = useCounterStore();
const channelStore = useChannelStore();

// 直接解构会丢失响应式
// const { count, doubleCount } = counterStore;
// 需要用storeToRefs方法包装一下
const {count, doubbleCount} = storeToRefs(counterStore);
//方法无所谓
const { increment,decrement } = counterStore;
</script>
```


## 14.4 Pinia持久化插件
在vuex中，如果需要做持久化，都是使用localStorage.getItem() . localStorage.setItem() 自己编写。


![[Pasted image 20250118205556.png]]


具体步骤，看官网[Pinia Plugin Persistedstate](https://prazdevs.github.io/pinia-plugin-persistedstate/zh/)