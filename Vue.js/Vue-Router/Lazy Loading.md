> We can implement the lazy loading easily, thanks to an upcoming JavaScript feature, [dynamic imports](https://developers.google.com/web/updates/2017/11/dynamic-import), that [webpack supports](https://webpack.js.org/guides/code-splitting/#dynamic-imports). Currently, the `src/router.js` file has the following code: 



```javascript
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)

function loadView(view) {
  return () => import(/* webpackChunkName: "view-[request]" */ `@/views/${view}.vue`)
}

export default new Router({
  routes: [
    {
      path: '/',
      name: 'home',
      component: loadView('Home')
    },
    {
      path: '/about',
      name: 'about',
      component: loadView('About')
    }
  ]
})
```

