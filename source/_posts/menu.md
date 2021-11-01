---
title: vue 动态菜单那些事
date: 2019-02-01 19:23:24
index_img: img/menu.png
tags: vue element  
categories: vue element  
---

自己独立搭建vue 动态菜单逻辑

![1635496770668](1635496770668.png)

先在数据库字段的增删改  

![1635496838883](1635496838883.png)

为角色分配菜单

![1635496905871](1635496905871.png)

配置异步菜单本地为对象  名称唯一

```
import { asyncRoutes, constantRoutes } from '@/router'
import { fetchRoleMenu } from '@/api/equipment/role'
import { fetchDetail } from '@/api/equipment/menu'

function initMenufield(arr = [], userMenuId = []) {
  arr.forEach((t, i) => {
    t.hidden = !t.visible
    t.name = t.menuName
    t.component = asyncRoutes[t.component]
    t.meta = { title: t.name, icon: t.icon }
    if (t.children && t.children.length > 0) {
      initMenufield(t.children, userMenuId)
    }
  })
}
// eslint-disable-next-line no-unused-vars
<!-- function removeMenu(arr = [], userMenuId = []) {
  arr.forEach((t, i) => {
    if (userMenuId.includes(t.menuId) && (t.endable == 1)) {
      if (t.children && t.children.length > 0) {
        removeMenu(t.children, userMenuId)
      }
    } else {
      arr.splice(i, 1)
    }
  })
} -->
//动态下标删除问题
function removeMenu(arr = [], userMenuId = []) {
  for (let i = 0; i < arr.length; i++) {
    if (userMenuId.includes(arr[i].menuId) && (arr[i].endable == 1)) {
      if (arr[i].children && arr[i].children.length > 0) {
        removeMenu(arr[i].children, userMenuId)
      }
    } else {
      arr.splice(i, 1)
      i--
    }
  }
}
export async function filterAsyncRoutes(routes, roles, commit, resolve) {
  const { data: allMenu } = await fetchDetail({})
  const { objs } = await fetchRoleMenu({ roleId: localStorage.getItem('roleIds') })
  let userMenuId = []
  try {
    userMenuId = (JSON.parse(objs.cjson))?.menuids || []
  } catch (error) {
    console.log(error)
  }
  removeMenu(allMenu, userMenuId)
  // 移除路由 知道路径也进不去
  // console.table(allMenu)
  initMenufield(allMenu, userMenuId)
  // console.table(allMenu)
  // 生成路由

  allMenu.push(
    { path: '*', redirect: '/dashboard', hidden: true }
  )
  let res = []
  res = allMenu
  commit('SET_ROUTES', res)
  resolve(res)
}

const state = {
  routes: [],
  addRoutes: []
}

const mutations = {
  SET_ROUTES: (state, routes) => {
    state.addRoutes = routes
    state.routes = constantRoutes.concat(routes)
  }
}

const actions = {
  generateRoutes({ commit }, roles) {
    return new Promise(resolve => {
      filterAsyncRoutes(asyncRoutes, roles, commit, resolve)
    })
  }
}

export default {
  namespaced: true,
  state,
  mutations,
  actions
}

```

1内部先拉取全部树形菜单   

2根据角色拉取自己菜单id

3 移除无用菜单

4 把剩余菜单对象转换

```
 t.hidden = !t.visible
    t.name = t.menuName
    t.component = asyncRoutes[t.component]
    t.meta = { title: t.name, icon: t.icon }
    if (t.children && t.children.length > 0) {
      initMenufield(t.children, userMenuId)
    }
```

不启用代表没有此菜单

不展示代表菜单不展示可以跳转 比如详情类页面需要id

![1635497346785](1635497346785.png)

1级菜单必须配置

![1635497383715](1635497383715.png)

二级菜单配置自己的组件

内部要包含

```
  <!-- 路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
```

![1635497475678](1635497475678.png)