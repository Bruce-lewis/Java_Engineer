
后端在线接口：[提交订单 - 小兔鲜儿-小程序版](https://www.apifox.cn/apidoc/shared-0e6ee326-d646-41bd-9214-29dbf47648fa/api-43426940)

在线代码笔记：[小兔鲜儿 - 项目起步 | uniapp+vue3+ts](https://megasu.atomgit.net/uni-app-shop-note/rabbit-shop/)


![[Pasted image 20250222143039.png]]

# 一. 通过命令行方式创建uni-app项目

vue3+ts版本：
npx degit dcloudio/uni-preset-vue#vite-ts my-vue3-project
官网链接：
https://uniapp.dcloud.net.cn/quickstart-cli.html#创建uni-app

# 二. 使用VSCode开发
![[Pasted image 20250222212149.png]]
![[Pasted image 20250222215434.png]]

pnpm i @types/wechat-miniprogram @uni-helper/uni-app-types

tsconfig.json
```json
{
  "extends": "@vue/tsconfig/tsconfig.json",
  "compilerOptions": {
    "sourceMap": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    },
    "lib": ["esnext", "dom"],
    "types": ["@dcloudio/types","@types/wechat-miniprogram","@uni-helper/uni-app-types"]
  },
  "vueCompilerOptions":{
    "experimentalRuntimeMode":"runtime-uni-app"
  },
  "include": ["src/**/*.ts", "src/**/*.d.ts", "src/**/*.tsx", "src/**/*.vue"]
}
```


# 三. 拉取项目
![[Pasted image 20250222215941.png]]
git clone http://git.itcast.cn/heimaqianduan/erabbit-uni-app-vue3-ts.git heima-shop


# 08. 基础架构-安装uni-ui组件库
![[Pasted image 20250228123653.png]]
安装uni-ui
pnpm i @dcloudio/uni-ui


去官网引入ui组件
[uni-app官网](https://uniapp.dcloud.net.cn/component/uniui/uni-card.html)


引入uni-ui类型
pnpm i -D @uni-helper/uni-ui-types
[@uni-helper/uni-ui-types - npm](https://www.npmjs.com/package/@uni-helper/uni-ui-types)

# 9. 基础架构-小程序端Pinia持久化
![[Pasted image 20250228125758.png]]

# 10. 基础架构-请求和上传文件拦截器
![[Pasted image 20250228125824.png]]
![[Pasted image 20250228130120.png]]
![[Pasted image 20250228160625.png]]
# 11. 基础架构-请求函数封装-请求成功
![[Pasted image 20250228161249.png]]
# 12. 基础架构-请求函数封装-请求失败
![[Pasted image 20250228165315.png]]
![[Pasted image 20250228165446.png]]
![[Pasted image 20250228175948.png]]


# 13 首页-自定义导航栏
![[Pasted image 20250228182704.png]]