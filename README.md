# vue-quick-start

## 安装
```
npm install
```

### 本地测试
```
npm run serve
```

### 编译打包
```
npm run build
```

# 快速开始

## 1、新增一个页面

在 `/src/views/` 目录下新建 `About.vue` 文件，初始内容为:

```
<template>
    <div id="about"><!-- 页面内容 --></div>
</template>

<script>
export default {
    name: 'About'
};
</script>

<style lang="less" scoped>
#about {
    width: 100%;
}
</style>
```

## 2、注册到路由

打开 `src/router.js`，在 `routes` 路由数组内加入 `About` 路由: 

```
export default new Router({
    routes: [{
        path: '/',
        name: 'index',
        component: component: () => import('./views/Index')
    }, {
        path: '/about',
        name: 'about',
        component: component: () => import('./views/About')
    }]
})
```

## 3、函数式跳转到 About 页面

如在 `Index` 页面的 `created()` 生命周期内加入 `this.$router.push('/about')`，那么当 `Index` 页面加载时候就会跳转到 `About` 页面

```
<script>
export default {
    name: 'Index',
    created() {
        this.$router.push('/about')
    }
};
</script>
```

# HTTP 请求

已配置 `vue-axios` 

使用方法: https://github.com/imcvampire/vue-axios#readme

# ICON 矢量图标库

已配置 `vue-awesome`

使用方法: https://justineo.github.io/vue-awesome/demo/

图标列表: http://www.fontawesome.com.cn/faicons/