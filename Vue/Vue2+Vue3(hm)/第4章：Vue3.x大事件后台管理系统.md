![[Pasted image 20250118211438.png]]

# 1. pnpm 新一代包管理工具
![[Pasted image 20250118211856.png]]


# 2. Eslint & prettier 配置代码风格

**环境同步：**

1. **安装了插件 ESlint，开启保存自动修复**
2. **禁用了插件 Prettier，并关闭保存自动格式化**



由于vue3以及ESlint和Prettier插件的升级，无法参考学习视频，
当前我的项目
==.prettierrc.json== 文件 是用于格式化代码的
==eslint.config.js== 是用于语法检查的。


# 3. 基于 husky  的代码检查工作流

husky 是一个 git hooks 工具  ( git的钩子工具，可以在特定时机执行特定的命令 )

**husky 配置**

1. git初始化 git init

2. 初始化 husky 工具配置  https://typicode.github.io/husky/

```jsx
pnpm dlx husky-init && pnpm install
```

3. 修改 .husky/pre-commit 文件

```jsx
pnpm lint
```

**问题：**默认进行的是全量检查，耗时问题，历史问题。



**lint-staged 配置**

1. 安装

```jsx
pnpm i lint-staged -D
```

2. 配置 `package.json`

```jsx
{
  // ... 省略 ...
  "lint-staged": {
    "*.{js,ts,vue}": [
      "eslint --fix"
    ]
  }
}

{
  "scripts": {
    // ... 省略 ...
    "lint-staged": "lint-staged"
  }
}
```

3. 修改 .husky/pre-commit 文件

```jsx
pnpm lint-staged
```






# 4. 目录调整
![[Pasted image 20250119131238.png]]
安装预处理器：pnpm add sass -D

![[Pasted image 20250119134753.png]]

# 5. vue-router4

![[Pasted image 20250119135309.png]]

如何实现编程式路由导航？
![[Pasted image 20250119135252.png]]

# 6. 引入Element Plus组件库
![[Pasted image 20250119140226.png]]

# 7. Pinia构建仓库和 持久化



 
 ![[Pasted image 20250119141304.png]]
![[Pasted image 20250119141416.png]]


# 8. 请求交互-请求工具设计
![[Pasted image 20250119141538.png]]

pnpm add axios



# 9. 路由设计
