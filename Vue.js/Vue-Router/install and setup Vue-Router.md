

# How to install and setup Vue-Router

1. Run the command: `npm install --save vue-router`

2. Create a new `js` file (in my case: `routes.js`) like :

   ```javascript
   import Vue from 'vue'
   import Router from 'vue-router'
   import Homepage from '@/pages/home/Home.vue'
   
   Vue.use(Router)
   
   export const router = new Router({
     routes: [
       {
         path: '/',
         name: 'Homepage',
         component: Homepage,
       },
   	//other routes
     ],
     mode: 'history',
   })
   ```

3. Then in `main.js` you have to import the above file and and inject the router option to vue.js instance:

   ```javascript
   import Vue from 'vue'
   import App from './App.vue'
   import  { router } from './routes'
   
   new Vue({
     el: '#app',
     router,
     render: h => h(App)
   })
   ```