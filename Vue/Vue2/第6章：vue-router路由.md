vue-router是vue的一个插件库，专门用来实现spa应用（single page web application）页面的跳转
![[Pasted image 20250115124253.png]]


概念：一个路由就是一组映射关系（key-value，key是路径，value是组件），多个路由需要路由器（router）进行管理。


# 6.1 路由的基本使用



![[Pasted image 20250115215732.png]]
![[Pasted image 20250115215753.png]]


实现效果
![[Pasted image 20250115223006.png]]
## 1. 引入依赖
vue-router4只能在vue3中使用，
vue-router3才能在vue2中使用。
```shell
npm i vue-router@3
```

## 2. 应用router插件
main.js
```js
import Vue from 'vue'
import App from './App.vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)
Vue.config.productionTip = false
const vm = new Vue({
  render: h => h(App),

}).$mount('#app')

```

## 3. 创建router并配置route规则

src/router/index.js
```js
// 引入VueRouter
import VueRouter from 'vue-router'
// 引入组件
import About from '../route-pages/About.vue'
import Home from '../route-pages/Home.vue'

// 创建并router，配置路由映射规则
const router = new VueRouter({
  routes: [
    {
      path: '/about',
      component: About
    },
    {
      path: '/home',
      component: Home
    }
  ]
})
export default router
```

## 4. 配置router
main.js
```js
import Vue from 'vue'
import App from './App.vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)
Vue.config.productionTip = false
const vm = new Vue({
  render: h => h(App),
	router:router,
}).$mount('#app')

```


## 5. 应用路由
App.vue
```vue
<template>
  <div id="app" class="container">
    <div class="navigator" :style="{ width: '30%' }">
      <!-- <a href="">About</a>
      <hr />
      <a href="">Home</a> -->

      <router-link to="/about" active-class="active">About</router-link>
      <hr />
      <router-link to="/home" active-class="active">Home</router-link>
    </div>
    <div class="route-page" :style="{ width: '69%' }">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
};
</script>

<style>
.container {
  display: flex;
  justify-content: space-around;
}
</style>

```

# 6.2 几个注意点
![[Pasted image 20250115221830.png]]



# 6.3 嵌套（多级）路由
![[Pasted image 20250115222831.png]]
实现效果
![[Pasted image 20250115223050.png]]

## 1. 准备组件
Home.vue
```vue
<template>
  <div>
    <h3>This is Home page</h3>

    <router-link to="/home/news" active-class="active">News</router-link
    ><span>&nbsp;</span>
    <router-link to="/home/message" active-class="active">Message</router-link>

    <div class="route-pages">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
};
</script>

<style></style>

```

Message.vue
```vue
<template>
  <div>
    <h1>消息页面</h1>
    <ul>
      <li>消息1</li>
      <li>消息2</li>
      <li>消息3</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Message",
};
</script>

<style></style>

```

News.vue
```vue
<template>
  <div>
    <h1>新闻页面</h1>
    <ul>
      <li>新闻1</li>
      <li>新闻2</li>
      <li>新闻3</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "News",
};
</script>

<style></style>

```


## 2. 编写路由规则
src/router/index.js
```js
// 引入VueRouter
import VueRouter from 'vue-router'
// 引入组件
import About from '../route-pages/About.vue'
import Home from '../route-pages/Home.vue'
import News from '../route-pages/News.vue'
import Message from '../route-pages/Message.vue'

// 创建并router，配置路由映射规则
const router = new VueRouter({
  routes: [
    {
      path: '/about',
      component: About
    },
    {
      path: '/home',
        component: Home,
      children: [
        {
          path: 'message',
          component: Message
        },
        {
          path: 'news',
          component:News
        }
      ]
    }
  ]
})
export default router
```

# 6.4 路由传参
![[Pasted image 20250115225224.png]]
## 6.4.1 query 传参
Message.vue
```vue
<template>
  <div>
    <h1>消息页面</h1>
    <ul>
      <li v-for="item in messageList" :key="item.id">
        <!-- 跳转路由并携带query参数，to的字符串写法 -->
        <!-- <router-link
          :to="`/home/message/detail?id=${item.id}&title=${item.title}&content=${item.content}`"
        >
          {{ item.title }}
        </router-link> -->

        <!-- 跳转路由并携带query参数，to的对象写法 -->
        <router-link
          :to="{
            path: '/home/message/detail',
            query: { id: item.id, title: item.title, content: item.content },
          }"
        >
          {{ item.title }}
        </router-link>
      </li>
    </ul>

    <hr />
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: "Message",
  data() {
    return {
      messageList: [
        { id: "001", title: "消息1", content: "消息内容1" },
        { id: "002", title: "消息2", content: "消息内容2" },
        { id: "003", title: "消息3", content: "消息内容3" },
      ],
    };
  },
};
</script>

<style></style>

```

src/router/index.js
```js
// 引入VueRouter
import VueRouter from 'vue-router'
// 引入组件
import About from '../route-pages/About.vue'
import Home from '../route-pages/Home.vue'
import News from '../route-pages/News.vue'
import Message from '../route-pages/Message.vue'
import Detail from '../route-pages/Detail.vue'

// 创建并router，配置路由映射规则
const router = new VueRouter({
  routes: [
    {
      path: '/about',
      component: About
    },
    {
      path: '/home',
        component: Home,
      children: [
        {
          path: 'message',
              component: Message,
          children: [
            {
              path: 'detail',
              component: Detail
            }
          ]
        },
        {
          path: 'news',
          component:News
        }
      ]
    }
  ]
})
export default router
```

Detail.vue
```vue
<template>
  <div>
    <div>
      消息标题：{{ $route.query.title }} <br />
      消息内容：{{ $route.query.content }}
    </div>
  </div>
</template>

<script>
export default {
  name: "Detail",
};
</script>

<style></style>

```

## 6.4.2 params传参
src/router/index.js
```js
// 引入VueRouter
import VueRouter from 'vue-router'
// 引入组件
import About from '../route-pages/About.vue'
import Home from '../route-pages/Home.vue'
import News from '../route-pages/News.vue'
import Message from '../route-pages/Message.vue'
import Detail from '../route-pages/Detail.vue'

// 创建并router，配置路由映射规则
const router = new VueRouter({
  routes: [
    {
      path: '/about',
      component: About
    },
    {
      path: '/home',
        component: Home,
      children: [
        {
          path: 'message',
              component: Message,
          children: [
            {
              name: 'detail',
              // 路径参数需要加冒号
              path: 'detail/:id/:title/:content',
              component: Detail
            }
          ]
        },
        {
          path: 'news',
          component:News
        }
      ]
    }
  ]
})
export default router
```


Message.vue
```vue
<template>
  <div>
    <h1>消息页面</h1>
    <ul>
      <li v-for="item in messageList" :key="item.id">
        <!-- 跳转路由并携带query参数，to的字符串写法 -->
        <!-- <router-link
          :to="`/home/message/detail?id=${item.id}&title=${item.title}&content=${item.content}`"
        >
          {{ item.title }}
        </router-link> -->

        <!-- 跳转路由并携带query参数，to的对象写法 -->
        <!-- <router-link
          :to="{
            path: '/home/message/detail',
            query: { id: item.id, title: item.title, content: item.content },
          }"
        >
          {{ item.title }}
        </router-link> -->

        <!-- 路由跳转 传递params参数, to的字符串写法 -->
        <!-- <router-link
          :to="`/home/message/detail/${item.id}/${item.title}/${item.content}`"
        >
          {{ item.title }}
        </router-link> -->

        <!-- 路由跳转 传递params参数, to的对象写法 -->
        <router-link
          :to="{
            //携带params参数，必须使用name跳转
            name: 'detail',
            params: { id: item.id, title: item.title, content: item.content },
          }"
        >
          {{ item.title }}
        </router-link>
      </li>
    </ul>

    <hr />
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: "Message",
  data() {
    return {
      messageList: [
        { id: "001", title: "消息1", content: "消息内容1" },
        { id: "002", title: "消息2", content: "消息内容2" },
        { id: "003", title: "消息3", content: "消息内容3" },
      ],
    };
  },
};
</script>

<style></style>

```

Detail.vue
```vue
<template>
  <div>
    <div>
      消息标题：{{ $route.params.title }} <br />
      消息内容：{{ $route.params.content }}
    </div>
  </div>
</template>

<script>
export default {
  name: "Detail",
};
</script>

<style></style>

```

## 6.4.3 路由的props配置
作用：让路由组件更加方便的接收参数。
![[Pasted image 20250116131541.png]]

src/router/index.js
```js
// 引入VueRouter
import VueRouter from 'vue-router'
// 引入组件
import About from '../route-pages/About.vue'
import Home from '../route-pages/Home.vue'
import News from '../route-pages/News.vue'
import Message from '../route-pages/Message.vue'
import Detail from '../route-pages/Detail.vue'

// 创建并router，配置路由映射规则
const router = new VueRouter({
  routes: [
    {
      path: '/about',
      component: About
    },
    {
      path: '/home',
        component: Home,
      children: [
        {
          path: 'message',
              component: Message,
          children: [
            {
              name: 'detail',
              // 路径参数需要加冒号
              path: 'detail/:id/:title/:content',
                  component: Detail,
                  props: true,// 将路由Params参数以props形式传到组件中
                  //props的第二种用法，用于query传参
                //   props($route) {
                //     return {id: $route.query.id, title: $route.query.title, content: $route.query.content}
              //}
            }
          ]
        },
        {
          path: 'news',
          component:News
        }
      ]
    }
  ]
})
export default router
```

Detail.vue
```vue
<template>
  <div>
    <div>
      消息标题：{{ title }} <br />
      消息内容：{{ content }}
    </div>
  </div>
</template>

<script>
export default {
  name: "Detail",
  props: ["title", "content"],
};
</script>

<style></style>

```

## 6.4.4 路由的replace属性
![[Pasted image 20250116131921.png]]

# 6.5 编程式路由导航
![[Pasted image 20250116132805.png]]
Home.vue
```vue
<template>
  <div>
    <h3>This is Home page</h3>

    <router-link to="/home/news" active-class="active">News</router-link
    ><span>&nbsp;</span>
    <router-link to="/home/message" active-class="active">Message</router-link>

    <button @click="goAbout">3s后跳转到About</button>
    <div class="route-pages">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  methods: {
    goAbout() {
      setTimeout(() => {
        this.$router.push("/about");
        // this.$router.replace("/about");
      }, 3000);
    },
  },
};
</script>

<style></style>

```

# 6.6 缓存路由组件
使用场景就是：某一个路由页面有用户输入，不想跳走了之后，输入被刷空了。
![[Pasted image 20250116133258.png]]
![[Pasted image 20250116133614.png]]

News.vue
```vue
<template>
  <div>
    <h1>新闻页面</h1>
    <ul>
      <li>新闻1</li>
      <li>新闻2</li>
      <li>新闻3</li>
    </ul>
    <!-- 大输入文本域 -->
    <textarea name="" id="" cols="30" rows="10"></textarea>
  </div>
</template>

<script>
export default {
  name: "News",
};
</script>

<style></style>

```
Home.vue
```vue
<template>
  <div>
    <h3>This is Home page</h3>

    <router-link to="/home/news" active-class="active">News</router-link
    ><span>&nbsp;</span>
    <router-link to="/home/message" active-class="active">Message</router-link>

    <button @click="goAbout">3s后跳转到About</button>
    <div class="route-pages">
      <!-- 缓存News组件，因为该组件中有用户输入的数据，另外Include中配置的是组件的名字 -->
      <keep-alive include="News">
        <router-view></router-view>
      </keep-alive>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  methods: {
    goAbout() {
      setTimeout(() => {
        this.$router.push("/about");
        // this.$router.replace("/about");
      }, 3000);
    },
  },
};
</script>

<style></style>

```

# 6.7 路由组件的特有的生命周期钩子

这个应用的场景，你将一个路由组件缓存了，但是你希望每次跳转到该路由组件时，初始化一些东西，跳走时再销毁一些东西。（但是由于你对该路由组件缓存了，mounted和beforeDestoryed不会在跳走时触发）

![[Pasted image 20250116134402.png]]

# 6.8 路由守卫（类似拦截器）
## 6.8.1 全局守卫
前置路由守卫&后置路由守卫

前置：鉴权
后置：修改页签标题

src/router/index.js
```js
// 引入VueRouter
import VueRouter from 'vue-router'
// 引入组件
import About from '../route-pages/About.vue'
import Home from '../route-pages/Home.vue'
import News from '../route-pages/News.vue'
import Message from '../route-pages/Message.vue'
import Detail from '../route-pages/Detail.vue'

// 创建并router，配置路由映射规则
const router = new VueRouter({
  routes: [
    {
      path: '/about',
          component: About,
      meta: {title: '关于'}
    },
    {
        meta: {title: '主页'},
      path: '/home',
        component: Home,
      children: [
          {
              meta: {title: '消息'},
          path: 'message',
              component: Message,
          children: [
              {
                meta: {title: '详情'},
              name: 'detail',
              // 路径参数需要加冒号
              path: 'detail/:id/:title/:content',
                  component: Detail,
                  props: true,// 将路由Params参数以props形式传到组件中
                  //props的第二种用法，用于query传参
                //   props($route) {
                //     return {id: $route.query.id, title: $route.query.title, content: $route.query.content}
                  //   }
                  meta: {isAuth: true} // 路由元信息:给程序员提供写一写额外配置的
            }
          ]
        },
          {
            meta: {title: '新闻'},
          path: 'news',
          component:News
        }
      ]
    }
  ]
})

router.beforeEach((to, from, next) => {
  // console.log(to, from);
  // if(to.path === '/home/message') {
  //   alert('请先登录')
  // } else {
  //   next()
    // }
    // 上面通过路径判断，更好的方式是通过路由元信息判断
    if (to.meta.isAuth) {
        if (localStorage.getItem('isLogin') === 'ok') {
            next()
        } else {
            alert('请先登录');
        }       
    } else {
        next();
    }
})

router.afterEach((to, from) => {
  // console.log('to', to);
  // console.log('from', from);
  document.title = to.meta.title || 'My System';
})

export default router
```


## 6.8.2 独享路由守卫
src/router/index.js
```js
// 引入VueRouter
import VueRouter from 'vue-router'
// 引入组件
import About from '../route-pages/About.vue'
import Home from '../route-pages/Home.vue'
import News from '../route-pages/News.vue'
import Message from '../route-pages/Message.vue'
import Detail from '../route-pages/Detail.vue'

// 创建并router，配置路由映射规则
const router = new VueRouter({
  routes: [
    {
      path: '/about',
          component: About,
      meta: {title: '关于'}
    },
    {
        meta: {title: '主页'},
      path: '/home',
        component: Home,
      children: [
          {
              meta: {title: '消息'},
          path: 'message',
              component: Message,
          children: [
              {
                meta: {title: '详情'},
              name: 'detail',
              // 路径参数需要加冒号
              path: 'detail/:id/:title/:content',
                  component: Detail,
                  props: true,// 将路由Params参数以props形式传到组件中
                  //props的第二种用法，用于query传参
                //   props($route) {
                //     return {id: $route.query.id, title: $route.query.title, content: $route.query.content}
                  //   }
                  meta: { isAuth: true }, // 路由元信息:给程序员提供写一写额外配置的
                  beforeEnter(to, from, next) {
                    if (to.meta.isAuth) {
                        if (localStorage.getItem('isLogin') === 'ok') {
                            next()
                        } else {
                            alert('请先登录');
                        }       
                    } else {
                        next();
                    }
                  }
                  
            }
          ]
        },
          {
            meta: {title: '新闻'},
          path: 'news',
          component:News
        }
      ]
    }
  ]
})

// router.beforeEach((to, from, next) => {
//   // console.log(to, from);
//   // if(to.path === '/home/message') {
//   //   alert('请先登录')
//   // } else {
//   //   next()
//     // }
//     // 上面通过路径判断，更好的方式是通过路由元信息判断
//     if (to.meta.isAuth) {
//         if (localStorage.getItem('isLogin') === 'ok') {
//             next()
//         } else {
//             alert('请先登录');
//         }       
//     } else {
//         next();
//     }
// })

router.afterEach((to, from) => {
  // console.log('to', to);
  // console.log('from', from);
  document.title = to.meta.title || 'My System';
})

export default router
```

## 6.8.3 组件内路由守卫
About.vue
```vue
<template>
  <div>This is About page</div>
</template>

<script>
export default {
  name: "About",

  // 通过路由规则进入该组件时调用
  beforeRouteEnter(to, from, next) {
    next();
  },
  //    通过路由规则离开该组件时调用
  beforeRouteLeave(to, from, next) {
    next();
  },
};
</script>

<style></style>

```
# 6.9 路由器的两种工作模式

![[Pasted image 20250116143833.png]]