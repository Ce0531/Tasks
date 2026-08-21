# Vue3学习笔记[^1]
[^1]:由于任务书未要求笔记，但我希望有笔记来总结知识点，往后可以及时复习巩固，加上暑假练车不能边看教学视频边用电脑做笔记，所以该笔记是通过AI总结得来，并非本人码出来的
## 一、Vue3 简介与框架概念
### 1. 什么是Vue
Vue 是一个**渐进式 JavaScript 框架**，用来构建用户界面。
- 渐进式：可以只在项目里用一小部分，也可以整个项目都用Vue
- 核心思想：**数据驱动视图**——数据变了，页面自动更新，不用手动操作DOM

### 2. Vue和原生JS的区别
|对比项|原生JS|Vue|
| ---- | ---- | ---- |
|操作页面|手动获取DOM、修改内容|修改数据，页面自动更新|
|代码量|多，重复操作多|少，声明式写法|
|维护性|页面复杂后难维护|组件化拆分，好维护|

### 3. Vue3相比Vue2的变化
- 性能更好，打包体积更小
- 新增 **Composition API（组合式API）**，逻辑可以抽离复用
- 更好的支持TypeScript

---

## 二、创建Vue3工程（Vite）
### 1. 环境准备
电脑需要先安装 **Node.js**，安装后自带 `npm` 命令。
- 检查是否安装：打开终端，输入 `node -v` 和 `npm -v`，有版本号就是装好了

### 2. 用命令行创建项目
在终端里依次执行：
```bash
# 1. 创建项目（my-vue-app 是项目名，可以自己改）
npm create vite@latest my-vue-app -- --template vue

# 2. 进入项目文件夹
cd my-vue-app

# 3. 安装依赖（第一次创建必须执行）
npm install

# 4. 启动项目（运行开发服务器）
npm run dev
```

执行 `npm run dev` 后，终端会给一个本地地址（一般是 `http://localhost:5173`），浏览器打开就能看到Vue页面。

### 3. 项目目录结构（重点认识）

```
my-vue-app
├── node_modules      # 依赖包，不用管
├── public            # 静态资源
├── src               # 我们写代码的地方，重点！
│   ├── assets        # 放图片、css等资源
│   ├── components    # 放公共组件
│   ├── App.vue       # 根组件，整个应用的入口组件
│   └── main.js       # 项目入口文件，挂载Vue应用
├── index.html        # 页面模板
├── package.json      # 项目配置和依赖信息
└── vite.config.js    # Vite配置文件
```

---

## 三、.vue 单文件组件（SFC）

### 1. 什么是单文件组件

一个 `.vue` 文件就是一个组件，里面包含三部分：

```
<template>
  <!-- HTML结构，写页面标签 -->
</template>

<script setup>
  // JS逻辑，写数据、方法
</script>

<style>
  /* CSS样式，写外观 */
</style>
```

- `<template>`：页面结构，**只能有一个根标签**（Vue3里其实可以多个，但新手建议包一个div）
- `<script setup>`：逻辑代码，用了语法糖之后写法更简单
- `<style>`：样式，加 `scoped` 属性可以让样式只在当前组件生效

### 2. 最简单的组件示例

```
<template>
  <div class="box">
    <h2>{{ title }}</h2>
    <button @click="sayHello">点我</button>
  </div>
</template>

<script setup>
// 定义数据
const title = '我的第一个Vue组件'

// 定义方法
function sayHello() {
  alert('你好Vue！')
}
</script>

<style scoped>
.box {
  padding: 20px;
  background: #f5f5f5;
}
</style>
```

### 3. 模板语法

#### (1) 插值表达式 `{{ }}`

把数据显示到页面上：

```
<template>
  <p>{{ name }}</p>
  <p>{{ 1 + 2 }}</p>
  <p>{{ isLogin ? '已登录' : '未登录' }}</p>
</template>
```

#### (2) 常用指令

| 指令 | 作用 | 写法示例 |
| --- | --- | --- |
| `v-bind` | 绑定属性（动态修改属性值） | `<img v-bind:src="imgUrl">` 简写 `:src="imgUrl"` |
| `v-on` | 绑定事件 | `<button v-on:click="fn">` 简写 `@click="fn"` |
| `v-if` | 条件渲染（满足才显示，不显示直接移除DOM） | `<div v-if="isShow">` |
| `v-show` | 条件显示（不显示用display:none隐藏） | `<div v-show="isShow">` |
| `v-for` | 列表渲染（循环生成元素） | `<li v-for="item in list" :key="item.id">{{item.name}}</li>` |
| `v-model` | 双向数据绑定（表单专用） | `<input v-model="username">` |

> 
> `v-if` vs `v-show`：频繁切换用v-show，不常切换用v-if

---

## 四、Options API 与 Composition API

### 1. Options API（选项式，Vue2写法）

把数据、方法、计算属性等分开写在不同选项里：

```
export default {
  data() { return { count: 0 } },
  methods: { add() { this.count++ } },
  computed: { double() { return this.count * 2 } }
}
```

- 特点：结构固定，新手好理解；但逻辑分散，复用麻烦

### 2. Composition API（组合式，Vue3推荐）

所有逻辑都写在 `setup` 里面，相关的代码可以放一起，方便抽离复用：

```
<script setup>
import { ref } from 'vue'
const count = ref(0)
function add() { count.value++ }
</script>
```

- 我们学习和写作业都用 **Composition API + script setup 语法糖**

---

## 五、setup 与 script setup 语法糖

### 1. setup 是什么

`setup` 是Vue3新增的配置项，是组合式API的入口，所有数据和方法都写在这里面。

### 2. script setup 语法糖（必学，项目主流写法）

在 `<script>` 标签上加 `setup`，写法更简洁：

```
<script setup>
// 直接定义变量和函数，不需要return，模板里直接能用
const msg = 'hello'
function fn() { console.log(msg) }
</script>
```

- 好处：不用写 `export default`、不用 `return`，代码更少
- **现在所有Vue3新项目都用这种写法**

---

## 六、响应式数据 ref 与 reactive

> 
> Vue的核心：数据变了页面自动更新。但普通变量不是响应式的，必须用ref或reactive包裹。

### 1. ref（基本类型 + 对象类型都能用）

```
<script setup>
import { ref } from 'vue'

// 基本类型
const count = ref(0)
// 注意：JS里修改和读取要加 .value
count.value++
console.log(count.value)

// 对象类型也可以用ref
const user = ref({ name: '张三', age: 18 })
user.value.name = '李四'
</script>

<template>
  <!-- 模板里不用加.value，自动解包 -->
  <p>{{ count }}</p>
  <p>{{ user.name }}</p>
</template>
```

### 2. reactive（只能包对象/数组类型）

```
<script setup>
import { reactive } from 'vue'

const user = reactive({
  name: '张三',
  age: 18
})
// 直接修改，不用加.value
user.name = '李四'
user.age++

const list = reactive([1, 2, 3])
list.push(4)
</script>

<template>
  <p>{{ user.name }} - {{ user.age }}</p>
</template>
```

### 3. ref 对比 reactive

| 对比项 | ref | reactive |
| --- | --- | --- |
| 支持类型 | 基本类型 + 对象类型 | 只能对象/数组 |
| JS里读写 | 要加 `.value` | 直接读写 |
| 模板里使用 | 自动解包，不用.value | 直接用 |
| 重新赋值 | 可以整体替换 | 不能整体替换（会失去响应式） |

> 
> 新手建议：**基本类型用ref，对象/数组用reactive**，不容易搞混。

---

## 七、toRefs 与 toRef

### 1. 为什么需要

从reactive对象里解构出来的值，会失去响应式：

```
const user = reactive({ name: '张三', age: 18 })
const { name, age } = user  // ❌ 这样解构出来不是响应式的
```

### 2. toRefs：把reactive对象的所有属性都变成响应式

```
import { reactive, toRefs } from 'vue'
const user = reactive({ name: '张三', age: 18 })
const { name, age } = toRefs(user)  // ✅ 解构出来还是响应式
// 使用时要加 .value
console.log(name.value)
```

### 3. toRef：只把某一个属性变成响应式

```
import { reactive, toRef } from 'vue'
const user = reactive({ name: '张三', age: 18 })
const name = toRef(user, 'name')  // 只拿name这一个
console.log(name.value)
```

---

## 八、computed 计算属性

### 1. 作用

根据已有数据，计算出一个新的值，**依赖的数据变了，它自动重新计算**。

```
<script setup>
import { ref, computed } from 'vue'
const firstName = ref('张')
const lastName = ref('三')

// 计算属性
const fullName = computed(() => {
  return firstName.value + lastName.value
})
</script>

<template>
  <p>{{ fullName }}</p>
</template>
```

### 2. 特点

- 有缓存：依赖不变时，多次访问直接用缓存结果，性能好
- 不能直接修改计算属性的值（默认只读）

---

## 九、watch 监视

### 1. 作用

监听某个数据的变化，数据一变就执行回调函数。

### 2. 监听ref数据

```
import { ref, watch } from 'vue'
const count = ref(0)

watch(count, (newVal, oldVal) => {
  console.log('变化了', '新值:', newVal, '旧值:', oldVal)
})
```

### 3. 监听reactive对象的某个属性

```
import { reactive, watch } from 'vue'
const user = reactive({ name: '张三', age: 18 })

// 监听单个属性，要用函数返回
watch(() => user.age, (newVal) => {
  console.log('年龄变了', newVal)
})
```

### 4. 监听多个数据

```
watch([count, () => user.age], ([newCount, newAge]) => {
  console.log('多个数据变化了')
})
```

### 5. 立即执行（immediate）

默认页面加载时不执行，数据变化才执行。加 `immediate: true` 可以一进来就执行一次：

```
watch(count, (newVal) => {
  console.log(newVal)
}, { immediate: true })
```

### 6. 深度监听（deep）

监听整个对象，对象内部任何属性变化都触发：

```
watch(user, (newVal) => {
  console.log('user变化了')
}, { deep: true })
```

---

## 十、标签的 ref 属性（获取DOM/组件实例）

### 1. 作用

和原生JS的 `document.getElementById` 类似，用来获取页面上的DOM元素或子组件实例。

### 2. 用法

```
<template>
  <!-- 给标签加 ref 属性，名字和JS里的变量名一致 -->
  <input ref="inputBox" type="text">
  <button @click="focusInput">让输入框获得焦点</button>
</template>

<script setup>
import { ref } from 'vue'
// 定义一个同名的ref变量
const inputBox = ref(null)

function focusInput() {
  // 通过 .value 拿到DOM元素
  inputBox.value.focus()
}
</script>
```

> 
> 注意：要在页面挂载完成之后才能拿到，`onMounted` 生命周期里用最保险。

---

## 十一、props 父子组件传值

### 1. 组件通信概念

组件拆分后，父组件要给子组件传数据，用 `props`。

### 2. 父组件传值

```
<!-- 父组件 Parent.vue -->
<template>
  <div>
    <!-- 通过属性把数据传给子组件 -->
    <Child :title="msg" :count="num"></Child>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Child from './Child.vue'
const msg = ref('我是父组件的数据')
const num = ref(100)
</script>
```

### 3. 子组件接收

```
<!-- 子组件 Child.vue -->
<template>
  <div>
    <h3>{{ title }}</h3>
    <p>{{ count }}</p>
  </div>
</template>

<script setup>
// 用defineProps接收父组件传过来的值
const props = defineProps({
  title: String,       // 规定类型
  count: {
    type: Number,
    default: 0         // 默认值
  }
})
// 模板里直接用 title、count；JS里用 props.title
</script>
```

> 
> props 是**只读**的，子组件不能直接修改父组件传过来的值。

---

## 十二、Vue Router 路由（⭐项目核心，实现页面跳转）

### 1. 什么是路由

路由就是**URL地址和页面对应关系**，不同的URL显示不同的页面组件，实现单页应用的页面切换（页面不刷新）。

### 2. 安装路由

```
npm install vue-router@4
```

### 3. 配置路由

在 `src` 下新建 `router/index.js`：

```
// src/router/index.js
import { createRouter, createWebHistory } from 'vue-router'

// 1. 引入页面组件
import Login from '../views/Login.vue'
import Home from '../views/Home.vue'

// 2. 配置路由规则：路径对应哪个组件
const routes = [
  { path: '/login', component: Login },
  { path: '/home', component: Home },
  // 默认重定向到登录页
  { path: '/', redirect: '/login' }
]

// 3. 创建路由实例
const router = createRouter({
  history: createWebHistory(),
  routes
})

export default router
```

### 4. 在 main.js 里挂载路由

```
// src/main.js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'  // 引入

const app = createApp(App)
app.use(router)  // 使用路由
app.mount('#app')
```

### 5. App.vue 里放路由出口

```
<!-- src/App.vue -->
<template>
  <!-- 路由匹配到的组件会显示在这里 -->
  <router-view></router-view>
</template>
```

### 6. 页面跳转的两种方式

#### (1) 声明式跳转（模板里用标签）

```
<template>
  <!-- to 属性写要跳转的路径 -->
  <router-link to="/home">去首页</router-link>
  <router-link to="/login">去登录页</router-link>
</template>
```

#### (2) 编程式跳转（JS里跳转，登录成功后跳转常用）

```
<script setup>
import { useRouter } from 'vue-router'
const router = useRouter()

function login() {
  // 登录成功后跳转到首页
  router.push('/home')
  // 也可以返回上一页
  // router.back()
}
</script>
```

### 7. 路由传参（了解）

```
// 跳转时传参
router.push({ path: '/home', query: { id: 123 } })

// 目标页面接收
import { useRoute } from 'vue-router'
const route = useRoute()
console.log(route.query.id)  // 123
```

---

## 十三、Pinia（替代Vuex，全局状态管理）⭐粗浅了解即可

### 1. 什么是Pinia

Vue3官方推荐的**全局状态管理工具**，作用是：多个组件共享同一份数据，比如登录后的用户信息，所有页面都能拿到。

> 
> 你任务里写的"Vuex"，Vue3新项目都用Pinia，作用一样，更简单。

### 2. 安装

```
npm install pinia
```

### 3. 挂载Pinia

```
// src/main.js
import { createPinia } from 'pinia'
const pinia = createPinia()
app.use(pinia)
```

### 4. 创建store（存数据的地方）

新建 `src/store/user.js`：

```
import { defineStore } from 'pinia'
import { ref } from 'vue'

// 定义一个store，第一个参数是名字（唯一）
export const useUserStore = defineStore('user', () => {
  // 定义全局数据
  const username = ref('游客')
  const token = ref('')

  // 定义修改数据的方法
  function setUser(name, t) {
    username.value = name
    token.value = t
  }

  function logout() {
    username.value = '游客'
    token.value = ''
  }

  // 必须return出去，外部才能用
  return { username, token, setUser, logout }
})
```

### 5. 在组件里使用

```
<script setup>
import { useUserStore } from '../store/user'
const userStore = useUserStore()

// 读取数据
console.log(userStore.username)

// 调用方法修改数据
userStore.setUser('张三', 'abc123')

// 也可以直接修改
userStore.username = '李四'
</script>

<template>
  <p>当前用户：{{ userStore.username }}</p>
</template>
```

### 6. 三种修改数据的方式

```
// 方式1：直接修改
userStore.username = '张三'

// 方式2：$patch批量修改多个
userStore.$patch({
  username: '张三',
  token: 'xxx'
})

// 方式3：调用store里定义的方法（推荐，逻辑清晰）
userStore.setUser('张三', 'xxx')
```

---

## 十四、Element-Plus 组件库

### 1. 什么是组件库

别人写好的现成UI组件（按钮、输入框、表单、弹窗等），直接拿来用，不用自己写CSS样式，开发快、界面统一。
Element-Plus 是Vue3最常用的组件库。

### 2. 安装

```
npm install element-plus
```

### 3. 全局引入（新手推荐，简单）

在 `main.js` 里：

```
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'  // 必须引入样式

app.use(ElementPlus)
```

### 4. 使用组件

直接在模板里写组件标签就行，所有组件都是 `el-` 开头：

```
<template>
  <div>
    <!-- 按钮 -->
    <el-button type="primary">主要按钮</el-button>
    <el-button type="success">成功按钮</el-button>
    <el-button type="danger">危险按钮</el-button>

    <!-- 输入框 -->
    <el-input v-model="username" placeholder="请输入用户名"></el-input>

    <!-- 表单 -->
    <el-form label-width="80px">
      <el-form-item label="用户名">
        <el-input v-model="form.username"></el-input>
      </el-form-item>
      <el-form-item label="密码">
        <el-input v-model="form.password" type="password"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submit">登录</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script setup>
import { reactive } from 'vue'
const form = reactive({
  username: '',
  password: ''
})
function submit() {
  console.log(form)
}
</script>
```

### 5. 常用组件清单

| 组件名 | 标签 | 作用 |
| --- | --- | --- |
| Button按钮 | `<el-button>` | 点击按钮 |
| Input输入框 | `<el-input>` | 文本输入 |
| Form表单 | `<el-form>` + `<el-form-item>` | 表单布局 |
| Message消息提示 | `ElMessage.success('登录成功')` | 弹出提示文字 |
| Dialog弹窗 | `<el-dialog>` | 弹窗 |
| Card卡片 | `<el-card>` | 卡片容器 |
| Menu菜单 | `<el-menu>` | 导航菜单 |

> 
> 更多组件直接查官方文档：element-plus.org，复制示例代码就能用。

---

## 十五、Vue3 生命周期（了解）

### 1. 常用生命周期钩子

| 钩子 | 触发时机 |
| --- | --- |
| `onBeforeMount` | 页面挂载之前 |
| `onMounted` | 页面挂载完成（DOM已经渲染好）⭐最常用，在这里发请求、操作DOM |
| `onBeforeUpdate` | 数据更新、页面重新渲染之前 |
| `onUpdated` | 页面重新渲染之后 |
| `onBeforeUnmount` | 组件销毁之前 |
| `onUnmounted` | 组件销毁之后 |

### 2. 用法

```
<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  console.log('页面加载完成了')
  // 在这里可以获取DOM、请求数据
})
</script>
```

---

## 十六、项目实战：登录页 + 自定义页（作业思路）

### 1. 项目结构

```
src
├── views
│   ├── Login.vue     # 登录注册页
│   └── Home.vue      # 自定义页面（比如首页/个人中心）
├── router
│   └── index.js      # 路由配置
├── store
│   └── user.js       # Pinia存用户信息
├── App.vue
└── main.js
```

### 2. 登录页 Login.vue（用Element-Plus）

```
<template>
  <div class="login-box">
    <el-card class="login-card">
      <h2>用户登录</h2>
      <el-form label-width="80px">
        <el-form-item label="用户名">
          <el-input v-model="form.username" placeholder="请输入用户名"></el-input>
        </el-form-item>
        <el-form-item label="密码">
          <el-input v-model="form.password" type="password" placeholder="请输入密码"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="login">登录</el-button>
          <el-button @click="toRegister">去注册</el-button>
        </el-form-item>
      </el-form>
    </el-card>
  </div>
</template>

<script setup>
import { reactive } from 'vue'
import { useRouter } from 'vue-router'
import { useUserStore } from '../store/user'
import { ElMessage } from 'element-plus'

const router = useRouter()
const userStore = useUserStore()

const form = reactive({
  username: '',
  password: ''
})

function login() {
  // 简单校验（不接后端，前端模拟）
  if (!form.username || !form.password) {
    ElMessage.warning('请输入用户名和密码')
    return
  }
  // 模拟登录成功，把用户信息存到Pinia
  userStore.setUser(form.username, 'mock-token-123')
  ElMessage.success('登录成功')
  // 跳转到首页
  router.push('/home')
}

function toRegister() {
  // 这里可以跳转到注册页，或者用弹窗
  ElMessage.info('注册功能自己扩展~')
}
</script>

<style scoped>
.login-box {
  display: flex;
  justify-content: center;
  padding-top: 100px;
}
.login-card {
  width: 400px;
}
</style>
```

### 3. 首页 Home.vue

```
<template>
  <div class="home">
    <h2>欢迎回来，{{ userStore.username }}</h2>
    <p>这是你的自定义页面</p>
    <el-button type="danger" @click="logout">退出登录</el-button>
    <el-button @click="goLogin">返回登录页</el-button>
  </div>
</template>

<script setup>
import { useRouter } from 'vue-router'
import { useUserStore } from '../store/user'
import { ElMessage } from 'element-plus'

const router = useRouter()
const userStore = useUserStore()

function logout() {
  userStore.logout()
  ElMessage.success('已退出登录')
  router.push('/login')
}

function goLogin() {
  router.push('/login')
}
</script>

<style scoped>
.home {
  padding: 40px;
  text-align: center;
}
</style>
```

### 4. 完成清单

- 命令行创建Vue项目（Vite）
- 使用 `.vue` 单文件模板
- 两个页面：登录注册页 + 自定义页面
- 页面之间互相跳转（Vue Router）
- 粗浅使用Pinia存用户信息
- 使用Element-Plus组件库


