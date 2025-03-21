Vue脚手架时官方提供的标准化开发工具（开发平台）

# 3.1 分析脚手架

使用步骤
①（仅在第一次使用执行）：全局安装@vue/cli
npm install -g @vue/cli

②切换到你要创建项目的目录，然后使用命令创建项目
vue create xxx

③启动项目
vue run serve

>[!info]
>1.如出现下载缓慢请配置 npm 淘宝镜像
npm config set registry https://registry.npm.taobao.org>
2.Vue 脚手架隐藏了所有 webpack相关的配置，若想査看具体的 webpakc 配置，请执行:vue inspect > output.js 

![[Pasted image 20250113153950.png]]




# 3.2 ref属性
![[Pasted image 20250113155121.png]]


App.vue
```vue
<template>
  <div id="app">
    <h1 ref="h1Dom">你好</h1>
    <School ref="school"></School>
    <button @click="getDomAndVc">获取DOM元素和组件实例vc</button>
  </div>
</template>

<script>
import School from "./components/School.vue";

export default {
  name: "App",
  components: {
    School,
  },
  methods: {
    getDomAndVc() {
      // 获取DOM元素
      const h1 = this.$refs.h1Dom;
      console.log(h1);
      // 获取组件实例
      const school = this.$refs.school;
      console.log(school);
    },
  },
};
</script>

<style></style>

```

School.vue
```vue
<template>
  <div class="school">
    <h3>学校名字:{{ name }}</h3>
    <h3>学校地址:{{ address }}</h3>
  </div>
</template>

<script>
export default {
  name: "School",
  data() {
    return {
      name: "尚硅谷",
      address: "北京",
    };
  },
};
</script>

<style>
.school {
  background-color: pink;
}
</style>

```

# 3.3 props 配置项（父传子首选）

![[Pasted image 20250113163314.png]]

App.vue
```vue
<template>
  <div id="app">
    <School name="尚硅谷" address="北京" :age="18" />
    <hr />
    <School name="黑马" address="上海" :age="20" />
    <hr />
    <School name="传智" address="深圳" />
    <hr />
  </div>
</template>

<script>
import School from "./components/School.vue";

export default {
  name: "App",
  components: {
    School,
  },
};
</script>

<style></style>

```


School.vue
```vue
<template>
  <div class="school">
    <h2>{{ title }}</h2>
    <h3>学校名字:{{ name }}</h3>
    <h3>学校地址:{{ address }}</h3>
    <h3>校龄：{{ local_age }}</h3>

    <!-- vue不建议修改props中的数据 -->
    <button @click="changeAge">增加校龄</button>
  </div>
</template>

<script>
export default {
  name: "School",
  data() {
    return {
      title: "学校信息",
      local_age: this.age,
    };
  },
  methods: {
    changeAge() {
      this.local_age++;
    },
  },
  //1.简单接收父组件传来的数据
  // props: ["name", "address", "age"],

  //2. 接收时限制类型
  // props: {
  //   name: String,
  //   address: String,
  //   age: Number,
  // },

  //3. 完整限制写法
  props: {
    name: {
      type: String,
      required: true,
      default: "传智播客",
    },
    address: {
      type: String,
      required: true,
      default: "深圳",
    },
    age: {
      type: Number,
      required: false,
      default: 20,
    },
  },
};
</script>

<style>
.school {
  /* background-color: pink; */
}
</style>

```

# 3.4 mixin混入
![[Pasted image 20250113164021.png]]

# 3.5 plugin 插件
![[Pasted image 20250113164706.png]]

main.js
```js
import Vue from 'vue'
import AppPlugin from  './app-plugin'
import App from './App.vue'

Vue.config.productionTip = false
//使用插件
Vue.use(AppPlugin);

const vm = new Vue({
  render: h => h(App),
}).$mount('#app')

console.log(vm);
```

app-plugin.js
```js
export default {
    install(Vue, options) {
        //全局过滤器
        Vue.filter('filterName', function (value) {
            // statements
        });

        //全局指令
        Vue.directive('my-directive', {
            bind: function (el, binding, vnode, oldVnode) {
                // statements
            }
        });

        //全局方法
        Vue.prototype.$myMethod = function (methodOptions) {
            // statements
        };

        //mixin混入
        Vue.mixin({
            created: function () {
                // statements
            }
        });
    }
}
```


# 3.6 Scoped 样式
scoped样式
作用:让样式在局部生效，防止冲突。
写法:\<style scoped>

# 3.7 Todo-List 案例
![[Pasted image 20250113174916.png]]

![[Pasted image 20250113205844.png]]

# 3.8 浏览器本地存储
![[Pasted image 20250113210731.png]]

# 3.9 组件的自定义事件（子传父首选）

给组件用的，不是给普通dom元素用的
![[Pasted image 20250114121331.png]]
App.vue
```vue
<template>
  <div id="app">
    <!-- 子给父传递数据：通过props配置项实现，父先给子传递函数 -->
    <School :getSchoolName="getSchoolName"></School>
    <hr />
    <!-- 子给父传递数据：通过给子组件绑定自定义事件实现（直接绑） -->
    <!-- <Student v-on:sendStudentNameEvent="getStudentName"></Student> -->
    <!-- 一次性事件 -->
    <!-- <Student v-on:sendStudentNameEvent.once="getStudentName"></Student> -->

    <!-- 子给父传递数据：通过给子组件绑定自定义事件实现(通过 ref) -->
    <Student ref="studentRef"></Student>
  </div>
</template>

<script>
import School from "./components/School.vue";
import Student from "./components/Student.vue";
export default {
  name: "App",
  components: {
    School,
    Student,
  },
  methods: {
    getSchoolName(name) {
      console.log("App收到学校名：", name);
    },
    getStudentName(name) {
      console.log("App收到学生名：", name);
    },
  },
  mounted() {
    //3s后绑定
    setTimeout(() => {
      this.$refs.studentRef.$on("sendStudentNameEvent", this.getStudentName);
      // this.$refs.studentRef.$once("sendStudentNameEvent", this.getStudentName);
    }, 3000);
  },
};
</script>

<style></style>

```

School.vue
```vue
<template>
  <div class="school">
    <h3>学校名字:{{ name }}</h3>
    <h3>学校地址:{{ address }}</h3>
    <button @click="sendNameToApp">给App传名字</button>
  </div>
</template>

<script>
export default {
  name: "School",
  props: ["getSchoolName"],
  data() {
    return {
      name: "尚硅谷",
      address: "北京",
    };
  },
  methods: {
    sendNameToApp() {
      this.getSchoolName(this.name);
    },
  },
};
</script>

<style scoped>
.school {
  background-color: pink;
}
</style>

```

Student.vue
```vue
<template>
  <div class="school">
    <h3>学生名字:{{ name }}</h3>
    <h3>学生地址:{{ address }}</h3>
    <button @click="sendNameToApp">给App传名字</button>
    <button @click="unbindEvent">解绑事件</button>
  </div>
</template>

<script>
export default {
  name: "Student",
  data() {
    return {
      name: "张三",
      address: "北京",
    };
  },
  methods: {
    sendNameToApp() {
      this.$emit("sendStudentNameEvent", this.name);
    },
    unbindEvent() {
      //解绑一个事件
      this.$off("sendStudentNameEvent");
      //解绑多个事件
      // this.$off(["sendStudentNameEvent","xxxEvent"]);
      this.$off();
    },
  },
};
</script>

<style scoped>
.school {
  background-color: aquamarine;
}
</style>

```

# 3.10全局事件总线（任意组件间通信首选）
![[Pasted image 20250114123605.png]]
①X要能被所有组件访问到，②X可以调用$on、 $off、 $emit方法
一个好的选择： 在Vue原型上定义一个变量（满足①），变量的类型是 vm、vc都行（满足②）
![[Pasted image 20250114125657.png]]
main.js
```js
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

const vm = new Vue({
  render: h => h(App),
  beforeCreate() {
    Vue.prototype.$bus = this // 全局事件总线
  }
}).$mount('#app')

```

App.vue
```vue
<template>
  <div id="app">
    <!-- 子给父传递数据：通过props配置项实现，父先给子传递函数 -->
    <School :getSchoolName="getSchoolName"></School>
    <hr />
    <!-- 子给父传递数据：通过给子组件绑定自定义事件实现（直接绑） -->
    <!-- <Student v-on:sendStudentNameEvent="getStudentName"></Student> -->
    <!-- 一次性事件 -->
    <!-- <Student v-on:sendStudentNameEvent.once="getStudentName"></Student> -->

    <!-- 子给父传递数据：通过给子组件绑定自定义事件实现(通过 ref) -->
    <Student ref="studentRef"></Student>
  </div>
</template>

<script>
import School from "./components/School.vue";
import Student from "./components/Student.vue";
export default {
  name: "App",
  components: {
    School,
    Student,
  },
  methods: {
    getSchoolName(name) {
      console.log("App收到学校名：", name);
    },
    getStudentName(name) {
      console.log("App收到学生名：", name);
    },
  },
  mounted() {
    //3s后绑定
    setTimeout(() => {
      this.$refs.studentRef.$on("sendStudentNameEvent", this.getStudentName);
      // this.$refs.studentRef.$once("sendStudentNameEvent", this.getStudentName);
    }, 3000);
  },
};
</script>

<style></style>

```

School.vue
```vue
<template>
  <div class="school">
    <h3>学校名字:{{ name }}</h3>
    <h3>学校地址:{{ address }}</h3>
  </div>
</template>

<script>
export default {
  name: "School",
  props: ["getSchoolName"],
  data() {
    return {
      name: "尚硅谷",
      address: "北京",
    };
  },
  methods: {
    getNameFromMyBrother(data) {
      console.log("I'm School, I have received a name from my brother:", data);
    },
  },
  mounted() {
    this.$bus.$on("sendNameEvent", this.getNameFromMyBrother);
  },
  beforeDestroy() {
    this.$bus.$off("sendNameEvent"); //解绑事件，不同于自定义组件（组件自己一旦销毁，绑定的事件自动销毁），全局事件总线的事件都是
    //绑定到Vue原型上的，所以需要手动解绑。
  },
};
</script>

<style scoped>
.school {
  background-color: pink;
}
</style>

```

Student.vue
```vue
<template>
  <div class="school">
    <h3>学生名字:{{ name }}</h3>
    <h3>学生地址:{{ address }}</h3>
    <button @click="sendNameToMyBrother">给兄弟组件传名字</button>
  </div>
</template>

<script>
export default {
  name: "Student",
  data() {
    return {
      name: "张三",
      address: "北京",
    };
  },
  methods: {
    sendNameToMyBrother() {
      this.$bus.$emit("sendNameEvent", this.name);
    },
  },
};
</script>

<style scoped>
.school {
  background-color: aquamarine;
}
</style>

```


# 3.11 消息订阅与发布

![[Pasted image 20250114130711.png]]
首先我们需要引入js库，pubsub.js 只是这种理念的其中一种实现

![[Pasted image 20250114131543.png]]

School.vue
```vue
<template>
  <div class="school">
    <h3>学校名字:{{ name }}</h3>
    <h3>学校地址:{{ address }}</h3>
  </div>
</template>

<script>
import pubsub from "pubsub-js";
export default {
  name: "School",
  props: ["getSchoolName"],
  data() {
    return {
      name: "尚硅谷",
      address: "北京",
    };
  },
  methods: {
    getNameFromMyBrother(messageName, data) {
      console.log(
        "I'm School, I have received a name from my brother:",
        messageName,
        data
      );
    },
  },
  mounted() {
    // this.$bus.$on("sendNameEvent", this.getNameFromMyBrother);
    const pubsubId = pubsub.subscribe(
      "sendNameMessage",
      this.getNameFromMyBrother
    );
  },
  beforeDestroy() {
    // this.$bus.$off("sendNameEvent"); //解绑事件，不同于自定义组件（组件自己一旦销毁，绑定的事件自动销毁），全局事件总线的事件都是
    //绑定到Vue原型上的，所以需要手动解绑。
    pubsub.unsubscribe(pubsubId); //解绑订阅。
  },
};
</script>

<style scoped>
.school {
  background-color: pink;
}
</style>

```

Student.vue
```vue
<template>
  <div class="school">
    <h3>学生名字:{{ name }}</h3>
    <h3>学生地址:{{ address }}</h3>
    <button @click="sendNameToMyBrother">给兄弟组件传名字</button>
  </div>
</template>

<script>
import pubsub from "pubsub-js";
export default {
  name: "Student",
  data() {
    return {
      name: "张三",
      address: "北京",
    };
  },
  methods: {
    sendNameToMyBrother() {
      // this.$bus.$emit("sendNameEvent", this.name);
      pubsub.publish("sendNameMessage", this.name);
    },
  },
};
</script>

<style scoped>
.school {
  background-color: aquamarine;
}
</style>

```



# 3.12 nextTick方法
![[Pasted image 20250114133126.png]]
```vue
<script>
...
editData(){
	this.isShowInput = true;//改变了data,但是vue不会立刻冲洗解析页面，会将代码走完才去解析。
	this.$refs.inputRef.focus();//不起效果，此时页面还没有input框
}
...
</script>
```
优化后
```vue
<script>
...
editData(){
	this.isShowInput = true;//改变了data,但是vue不会立刻冲洗解析页面，会将代码走完才去解析。
	//下一轮
	this.$nextTick(function(){
		this.$refs.inputRef.focus();//（模版重新解析后，执行）
	});
	
}
...
</script>
```


# 3.13 过渡和动画
我们不研究如何去写一个css动画效果，而是学习如何使用第三方库的
animate.css

[Animate.css | A cross-browser library of CSS animations.](https://animate.style/)
![[Pasted image 20250114135547.png]]
![[Pasted image 20250114135355.png]]