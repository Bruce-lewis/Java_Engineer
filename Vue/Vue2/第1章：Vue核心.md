# 1.1 Vue简介
vue是什么？
用于构建用户界面的渐进式框架。
==渐进式==（简单应用：只需引入轻量小巧的核心库，复杂应用：引入各种各样的Vue插件）

vue特点
1. 采用==组件化==（页面是由一个个组件构成，且能够复用）模式，提高代码的复用率。![[Pasted image 20250110144442.png]]
2. ==声明式==编码，开发人员无需直接操作DOM，提高开发效率。![[Pasted image 20250110144411.png]]
   3. 使用==虚拟DOM==+优秀的==Diff==算法，尽量复用DOM节点![[Pasted image 20250110144715.png]]
   
# 1.2 初识Vue
vue3的写法
```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <title>初始vue</title>
</head>

<body>
    <div id="root">
        <h1>{{ msg }}</h1>
    </div>
    <script type="text/javascript">
        const app = Vue.createApp({ // 创建 Vue 应用实例
            data() {
                return {
                    msg: 'hello, Bruce' // 返回数据对象
                }
            }
        });
        app.mount('#root'); // 挂载应用
    </script>
</body>

</html>

```

# 1.3 模版语法
```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <title>模版语法</title>
</head>

<body>
    <div id="root">
        <h1>插值语法</h1>
        <p>{{ msg }}</p>


        <h1>指令语法</h1>
        <a v-bind:href="url">去百度</a>
		<a :href="url">去百度(指令语法简写)</a>
    </div>
    <script type="text/javascript">
        const app = Vue.createApp({ // 创建 Vue 应用实例
            data() {
                return { // 返回数据对象
                    msg: 'hello, Bruce',
                    url: 'https://www.baidu.com'
                }
            }
        });
        app.mount('#root'); // 挂载应用
    </script>
</body>
</html>
```
### i. 插值语法   {{ }} 

### ii 指令语法  v-xxx: 
![[Pasted image 20250110173722.png]]
# 1.4 数据绑定

```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <title>数据绑定</title>
</head>

<body>

    <div id="root">
    单向数据绑定：<input type="text" v-bind:value="msg"><br/>
    双向数据绑定：<input type="text" v-model="msg2">
    </div>
    <script type="text/javascript">
        const app = Vue.createApp({ // 创建 Vue 应用实例
            data() {
                return { // 返回数据对象
                    msg: 'hello, Bruce',
                    msg2: 'hello, Bruce'
            
                }
            }
        });
        app.mount('#root'); // 挂载应用
    </script>
</body>
</html>
```

![[Pasted image 20250110174637.png]]
![[Pasted image 20250110175034.png]]
### i. 单向数据绑定
### ii. 双向数据绑定

# 1.5 MVVM模型

所以我们常用vm（ViewModel的缩写）这个变量名表示Vue实例。
![[Pasted image 20250110184145.png]]
![[Pasted image 20250110184620.png]]

# 1.6 数据代理

## 1. Object.defineProperty 方法

![[Pasted image 20250110190612.png]]
## 2. 何为数据代理
==数据代理==： 通过一个对象 代理 对 另一个对象中属性的操作（读/写）
```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <title>数据代理</title>
</head>

<body>

    <script type="text/javascript">
      let obj = {x:100}
      let proxyObj = {y:200}

      Object.defineProperty(proxyObj,'x',{
        get(){
          return obj.x
        },
        set(newValue){
          obj.x = newValue
        }
      })
    </script>
</body>
</html>
```
## 3. Vue中的数据代理
我的vue3没法演示，就用老师的vue2视频讲解吧

![[Pasted image 20250110193012.png]]
![[Pasted image 20250110192910.png]]
![[Pasted image 20250110193038.png]]

![[Pasted image 20250110193713.png]]

# 1.7 事件处理 @click
## 1. 事件的基本使用
```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <title>数据代理</title>
</head>

<body>
    <div id="root">
        <h1>{{ msg }}</h1>
        <button v-on:click="showInfo">点击展示信息</button>
        <button @click="showInfo">点击展示信息</button>
    </div>
    <script type="text/javascript">
        new Vue({
            el: '#root',
            data: {
                msg: 'hello, Bruce'
            },
            methods: {
                showInfo() {
                    alert('你点击了按钮');
                }
            }
        });
    </script>
</body>
</html>
```
![[Pasted image 20250110202720.png]]
## 2. 事件修饰符
```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <title>事件处理</title>
</head>

<body>
    <div id="root">
        <a href="http://www.baidu.com" @click.prevent="showInfo">点击展示信息</a>
    </div>
    <script type="text/javascript">
        new Vue({
            el: '#root',
            methods: {
                showInfo() {
                    alert('你点击了按钮');
                }
            }
        });
    </script>
</body>
</html>
```

![[Pasted image 20250110204021.png]]
## 3. 键盘事件
```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <title>键盘事件</title>
</head>

<body>
    <div id="root">
        <input type="text" placeholder="按下回车键,打印输入" @keyup.enter="showInfo">
    </div>
    <script type="text/javascript">
        new Vue({
            el: '#root',
            methods: {
                showInfo(event) {
                    console.log(event.target.value);
                }
            }
        });
    </script>
</body>
</html>
```
![[Pasted image 20250110204300.png]]

# 1.8 计算属性与监视 computed watch

## 1. 计算属性

```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <title>计算属性</title>
</head>

<body>
    <div id="root">
        <input type="text" v-model="firstName"/><br/>
        <input type="text" v-model="lastName"/><br/>
        <button @click="fullName = 'wang-wu'">修改fullName</button>
        <p>{{fullName}}</p>
    </div>
    <script type="text/javascript">
        new Vue({
            el: '#root',
            data () {
                return {
                    firstName: 'zhang',
                    lastName: 'san',
                }
            },
            computed: {
                //完整写法
                fullName:{
                    // getter, fullName属性被读取时，就会调用getter方法，方法的返回值就是fullName属性的值
                    // 什么时候调用？1：数据第一次被读取时。 2： 所依赖的数据发生变化时。
                    get(){
                        return this.firstName + '-' + this.lastName;
                    },
                    set(value){
                        const arr = value.split('-');
                        this.firstName = arr[0];
                        this.lastName = arr[1];
                    }
            
                }

                // 简写
                // fullName(){
                //     return this.firstName + '-' + this.lastName;
                // }
            }
        });
    </script>
</body>
</html>
```

## 2. 监视属性
```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <title>监视属性</title>
</head>

<body>
    <div id="root">
       当前温度：{{temperature}}°C 
       <button @click="temperature++">+</button>
    </div>
    <script type="text/javascript">
        new Vue({
            el: '#root',
            data () {
                return {
                    temperature: 20
                }
            },
            watch: {
                //简写
                // temperature (newValue, oldValue) {
                //     if(newValue>30){
                //         alert('温度过高！');
                //     }
                // }

                // 完整写法
                'temperature': {
                    handler (newValue, oldValue) {
                        if(newValue>30){
                            alert('温度过高！');
                        }
                    },
                    immediate: true //立即执行handler
                }
            },

        });
    </script>
</body>
</html>
```


深度监视
```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <title>监视属性</title>
</head>

<body>
    <div id="root">
      {{numbers.a}} <button @click="numbers.a++">+</button><br/>
      {{numbers.b}} <button @click="numbers.b++">+</button>

    </div>
    <script type="text/javascript">
        new Vue({
            el: '#root',
            data () {
                return {
                    numbers:{
                        a: 1,
                        b: 2
                    }
                }

            },
            watch: {
                'numbers': {
                    handler (newValue, oldValue) {
                        console.log('newValue', newValue);
                        console.log('oldValue', oldValue);
                    },
                    deep: true //深度监视
                }
            }
        });
    </script>
</body>
</html>
```

![[Pasted image 20250110222357.png]]
# 1.9 绑定样式 :class, :style
在应用界面，某些元素的样式是变化的。
class/style 绑定就是专门实现动态效果的技术。
## 1. class绑定
写法          :class='xxxx'
表达式是字符串：'classA'
表达式是对象：{classA: isA, classB: isB}
表达式是数组：['classA', 'classB']
![[Pasted image 20250111115735.png]]

## 2. style绑定
原生写法：\<div style='color: red; font-size=40px;'> 

表达式是对象
:style="{color:activeColor, fontSize: fontSize+'px'}"
表达式是数组
:style="[a,b]" 其中a、b是样式对象。


# 1.10 条件渲染 v-if, v-show
![[Pasted image 20250111121440.png]]

# 1.11 列表渲染 v-for

## 1. 基本使用
```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <title>Document</title>
</head>

<body>
    <div id="root">
        <!-- 遍历数组 -->
        <ul>
            <li v-for="(item,index) in list" :key="item.id">
                {{item.id}}- {{item.name}} - {{item.age}}
            </li>
        </ul>
        <!-- 遍历对象 -->
        <ul>
            <li v-for="(value,key) in people" :key="key">
                {{key}} - {{value}}
            </li>
        </ul>

    </div>
    <script type="text/javascript">
        new Vue({
            el: '#root',
            data() {
                return {
                    list: [
                        { id: '0001', name: '张三', age: 18 },
                        { id: '0002', name: '李四', age: 19 },
                        { id: '0003', name: '王五', age: 20 }
                    ],
                    people: {
                        id: '0004',
                        name: '赵六',
                        age: 21
                    }
                }
            },
        });
    </script>
</body>
</html>
```


## 2. key的原理
![[Pasted image 20250111124150.png]]

![[Pasted image 20250111124544.png]]


# ==数据改变---驱动--->视图更新==
（数据改变，Vue能渲染页面中数据被使用的地方)

总结：对于对象，能实现 页面动态改变的原理：①数据代理，②在代理对象的setter方法中修改页面中使用数据的地方。
![[Pasted image 20250111132308.png]]

对于数组
![[Pasted image 20250111134705.png]]
```js
// 示例数组
this.items = [1, 2, 3];

// 触发更新的方法
// 添加元素
this.items.push(4);       // 向末尾添加
this.items.unshift(0);    // 向头部插入
this.items.pop();         // 删除末尾元素
this.items.shift();       // 删除头部元素
this.items.splice(1, 1);  // 删除指定位置的元素（索引1处删1个）
this.items.reverse();     // 反转数组
this.items.sort();         // 排序数组
```

# 1.12 收集表单数据
```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <title>收集表单数据</title>
</head>

<body>
    <div id="root">
        <form @submit.prevent="submit">
            账号：<input type="text" v-model.trim="userInfo.account"></input><br />
            密码：<input type="password" v-model="userInfo.password"></input><br />
            年龄：<input type="number" v-model.number="userInfo.age"></input><br />
            性别：<input type="radio" v-model="userInfo.sex" value="男"></input>男
            <input type="radio" v-model="userInfo.sex" value="女"></input>女<br />
            爱好：
            <input type="checkbox" v-model="userInfo.hobby" value="吃饭"></input>吃饭
            <input type="checkbox" v-model="userInfo.hobby" value="睡觉"></input>睡觉
            <input type="checkbox" v-model="userInfo.hobby" value="打豆豆"></input>打豆豆<br />
            城市：
            <select v-model="userInfo.city">
                <option value="北京">北京</option>
                <option value="上海">上海</option>
                <option value="广州">广州</option>
            </select><br />
            备注：<textarea v-model.lazy="userInfo.remark"></textarea><br />
            <input type="checkbox" v-model="userInfo.isAgree"></input>同意协议
            <button>提交</button>
        </form>
    </div>
    <script type="text/javascript">
        new Vue({
            el: '#root',
            data() {
                return {
                    userInfo: {
                        account: '',
                        password: '',
                        age: 0,
                        sex: '男',
                        hobby: [],
                        city: '北京',
                        remark: '',
                        isAgree: false
                    }
                }
            },
            methods: {
                submit() {
                    console.log(this.userInfo);
                }
            }

        });
    </script>
</body>
</html>
```
![[Pasted image 20250111192653.png]]


# 1.13 过滤器

 稳定、快速、免费的前端开源项目 CDN 加速服务
http://bootcdn.cn


```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <script src="https://cdn.bootcdn.net/ajax/libs/dayjs/1.11.12/dayjs.min.js"></script>
    <title>过滤器</title>
</head>

<body>
    <div id="root">
        <h2>现在时间戳：{{nowTime}}</h2>
        <!-- 计算属性实现 -->
        <h2>格式化时间-计算属性：{{fmtTime}}</h2>
        <!-- method实现 -->
        <h2>格式化时间-method：{{fmtMethod()}}</h2>
        <!-- 过滤器实现 -->
        <h2>格式化时间-过滤器：{{nowTime | timeFilter}}</h2>
        <!-- 过滤器实现(传参) -->
        <h2>格式化时间-过滤器(传参)：{{nowTime | timeFilter('YYYY/MM/DD')}}</h2>
        <!-- 过滤器实现(多个过滤器) -->
        <h2>格式化时间-过滤器(多个过滤器)：{{nowTime | timeFilter('YYYY/MM/DD') | timeSlice }}</h2>
    </div>
    <script type="text/javascript">
        // 全局过滤器
        Vue.filter('globalSlice', function (value) {
            return value.slice(0, 4)
        })

        new Vue({
            el: '#root',
            data() {
                return {
                    nowTime: Date.now()
                }
            },
            computed: {
                fmtTime() {
                    return dayjs(this.nowTime).format('YYYY-MM-DD HH:mm:ss')
                }
            },
            methods: {
                fmtMethod() {
                    return dayjs(this.nowTime).format('YYYY-MM-DD HH:mm:ss')
                }
            }
            ,
            filters: {
                timeFilter(value, fmt = 'YYYY-MM-DD HH:mm:ss') {
                    return dayjs(value).format(fmt)
                },
                timeSlice(value) {
                    return value.slice(0, 4)
                }
            }
        });
    </script>
</body>

</html>
```

![[Pasted image 20250111195719.png]]

# 1.14 内置指令
![[Pasted image 20250111200252.png]]
## 1. v-text 
```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <title>内置指令</title>
</head>

<body>
    <div id="root">
        <div>{{msg}}</div>
        <div v-text="msg">I'll be repleced</div>
    </div>
    <script type="text/javascript">
        new Vue({
            el: '#root',
            data() {
                return {
                    msg: 'hello world'
                }
            },

        });
    </script>
</body>

</html>
```


## 2. v-html
```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <title>内置指令</title>
</head>

<body>
    <div id="root">
        <div>{{msg}}</div>
        <div v-html="msg">I'll be repleced</div>
    </div>
    <script type="text/javascript">
        new Vue({
            el: '#root',
            data() {
                return {
                    msg: '<h3>hello world</h3>'
                }
            },

        });
    </script>
</body>

</html>
```
## 3. v-cloak
![[Pasted image 20250111202358.png]]

## 4. v-once
![[Pasted image 20250111202704.png]]

## 5. v-pre
![[Pasted image 20250111202856.png]]

# 1.15 自定义指令

```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <title>自定义指令</title>
</head>

<body>
    <div id="root">
        <!-- 
        需求1:定义一个v-big指令，和v-text功能类似，但会把绑定的数值放大10倍。
        需求2:定义一个v-fbind指令，和v-bind功能类似，但可以让其所绑定的input元素默认获取焦点。 -->
        <h3>n的值为：<span v-text="n"></span></h3>
        <h3>n放大10倍后的值为: <span v-big="n"></span></h3>
        <button @click="n++">n+1</button>
        <hr>
        <input type="text" v-fbind="n"></input>
    </div>
    <script type="text/javascript">
        new Vue({
            el: '#root',
            data() {
                return {
                    n: 1
                }
            },
            directives: {
                //简写
                big(element, binding) {
                    element.innerText = binding.value * 10;
                },
                //完整写法
                fbind: {
                    //1. 指令与元素绑定时调用
                    bind(element, binding) {
                        element.value = binding.value;
                    },
                    //2. 指令所在元素被插入页面时调用
                    inserted(element) {
                        element.focus();
                    },
                    //3. 指令所在模板被重新解析时调用
                    update(element, binding) {
                        element.value = binding.value;
                    }
                }
            },

        });
    </script>
</body>

</html>
```
![[Pasted image 20250111205733.png]]

# 1.16 生命周期
![[Pasted image 20250111211436.png]]
![[Pasted image 20250111211812.png]]
![[Pasted image 20250111213503.png]]
 ![[Pasted image 20250111224208.png]]
