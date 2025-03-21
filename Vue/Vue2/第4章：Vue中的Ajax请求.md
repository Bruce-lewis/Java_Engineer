
# 4.1 axios

我们用的是axios

Axios 是一个基于 Promise 的 HTTP 客户端，用于浏览器和 Node.js。它可以用来发送 HTTP 请求，获取服务器的数据，支持各种请求方式（如 GET、POST、PUT、DELETE 等），并且提供了一些便利的功能，如请求和响应拦截器、请求取消、自动转换 JSON 数据等。


App.vue
```vue
<template>
  <div id="app">
    <SerachGitHubUsersVue></SerachGitHubUsersVue>
  </div>
</template>

<script>
import SerachGitHubUsersVue from "./components/SerachGitHubUsers.vue";
export default {
  name: "App",
  components: { SerachGitHubUsersVue },
};
</script>

<style></style>

```
SerachGitHubUsers.vue
```vue
<template>
  <div>
  Search Github Users:
    <input type="text" v-model="searchName" />
    <button @click="searchUser">搜索</button>
    <hr />
    <ul>
      <li v-for="item in users" :key="item.id">
        <a :href="item.html_url" target="_blank">
          <img :src="item.avatar_url" alt="" style="width: 100px" />
        </a>
        <p>{{ item.login }}</p>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "SerachGitHubUsers",
  data() {
    return {
      searchName: "",
      users: [],
    };
  },
  methods: {
    searchUser() {
      axios
        .get("https://api.github.com/search/users", {
          params: {
            q: this.searchName,
          },
        })
        .then((response) => {
          this.users = response.data.items;
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>

<style></style>

```


# 4.2 Slot 插槽
作用：让父组件可以向子组件==指定位置== 插入==html结构==

## 4.2.1 默认插槽
![[Pasted image 20250114152157.png]]
App.vue
```vue
<template>
  <div id="app" class="container">
    <Category title="美食">
      <!-- 渲染一个网络美食图片 -->
      <img src="https://img.yzcdn.cn/vant/apple-1.jpg" alt="" />
    </Category>
    <Category title="游戏">
      <ul>
        <li v-for="(item, index) in games" :key="index">
          <span>{{ item }}</span>
        </li>
      </ul>
    </Category>
    <Category title="电影">
      <!-- 渲染一个电影视频 -->
      <video
        src="https://www.w3school.com.cn/example/html5/mov_bbb.mp4"
        controls
      ></video>
    </Category>
  </div>
</template>

<script>
import Category from "./components/Category.vue";

export default {
  name: "App",
  components: { Category },
  data() {
    return {
      games: ["王者荣耀", "和平精英", "穿越火线"],
    };
  },
};
</script>

<style>
.container {
  display: flex;
  justify-content: space-around;
}
</style>

```

Category.vue
```vue
<template>
  <div class="category">
    <h3>{{ title }}</h3>
    <!-- 插槽: 等着组件使用者进行填充 -->
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "Category",
  props: {
    title: {
      type: String,
      default: "默认标题",
    },
  },
};
</script>

<style scoped>
.category {
  width: 200px;
  height: 300px;
}
.category h3 {
  background-color: coral;
}
img {
  width: 100%;
}
</style>

```

## 4.2.2 具名插槽
![[Pasted image 20250114153051.png]]

App.vue
```vue
<template>
  <div id="app" class="container">
    <Category title="美食">
      <!-- 渲染一个网络美食图片 -->
      <img slot="center" src="https://img.yzcdn.cn/vant/apple-1.jpg" alt="" />
      <a slot="footer" href="">更多美食</a>
    </Category>
    <Category title="游戏">
      <ul slot="center">
        <li v-for="(item, index) in games" :key="index">
          <span>{{ item }}</span>
        </li>
      </ul>
      <template slot="footer">
        <a href="">热门推荐</a>
        <a href="">更多游戏</a>
      </template>
    </Category>
    <Category title="电影">
      <!-- 渲染一个电影视频 -->
      <video
        slot="center"
        src="https://www.w3school.com.cn/example/html5/mov_bbb.mp4"
        controls
      ></video>
      <a slot="footer" href="">更多电影</a>
    </Category>
  </div>
</template>

<script>
import Category from "./components/Category.vue";

export default {
  name: "App",
  components: { Category },
  data() {
    return {
      games: ["王者荣耀", "和平精英", "穿越火线"],
    };
  },
};
</script>

<style>
.container {
  display: flex;
  justify-content: space-around;
}
</style>

```

Category.vue
```vue
<template>
  <div class="category">
    <h3>{{ title }}</h3>
    <!-- 插槽: 等着组件使用者进行填充 -->
    <slot name="center"></slot>

    <div>我是公共div</div>
    <slot name="footer"></slot>
  </div>
</template>

<script>
export default {
  name: "Category",
  props: {
    title: {
      type: String,
      default: "默认标题",
    },
  },
};
</script>

<style scoped>
.category {
  width: 200px;
  height: 300px;
}
.category h3 {
  background-color: coral;
}
img {
  width: 100%;
}
</style>

```

## 4.2.3 作用域插槽

![[Pasted image 20250114154858.png]]
App.vue
```vue
<template>
  <div id="app" class="container">
    <Category title="美食">
      <template scope="scopeData">
        <ul>
          <li v-for="(item, index) in scopeData.games" :key="index">
            <span>{{ item }}</span>
          </li>
        </ul>
      </template>
    </Category>
    <Category title="游戏">
      <template slot-scope="scopeData">
        <ol>
          <li v-for="(item, index) in scopeData.games" :key="index">
            <span>{{ item }}</span>
          </li>
        </ol>
      </template>
    </Category>
    <Category title="电影">
      <template slot-scope="{ games }">
        <h4 v-for="(item, index) in games" :key="index">
          <span>{{ item }}</span>
        </h4>
      </template></Category
    >
  </div>
</template>

<script>
import Category from "./components/Category.vue";

export default {
  name: "App",
  components: { Category },
};
</script>

<style>
.container {
  display: flex;
  justify-content: space-around;
}
</style>

```

Category.vue
```vue
<template>
  <div class="category">
    <h3>{{ title }}</h3>

    <slot :games="games"></slot>
  </div>
</template>

<script>
export default {
  name: "Category",
  props: {
    title: {
      type: String,
      default: "默认标题",
    },
  },
  data() {
    return {
      games: ["王者荣耀", "和平精英", "穿越火线"],
    };
  },
};
</script>

<style scoped>
.category {
  width: 200px;
  height: 300px;
}
.category h3 {
  background-color: coral;
}
img {
  width: 100%;
}
</style>

```

