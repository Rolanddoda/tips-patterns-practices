> ## How to create an axios instance including `Authorization` headers



1. Create a `js` file (i named it `request.js`)  and write this code:

   ```javascript
   import axios from 'axios'
   import { store } from "../../store/store" //=>we need the store to access the token
   
   const request = axios.create({
     baseURL: 'http://localhost:8000' //=> the base URL
   })
   
   /*
   Below we use a request interceptor in order to add an authorization header
   or not,regarding the state of token.
    */
   request.interceptors.request.use(request => {
     const token = store.state.token
     if (token) {
       request.headers.Authorization = 'Bearer ' + token
     }
     return request
   
   }, error => {
     return Promise.reject(error)
   })
   
   export default request
   ```

2. Now, we can import this file in our components like this

   `import request from './request'`

   And use like:

   `request.get('/users') //due to this axios intance we don't have to write all the url`

   But i like a better method.For all the requests i use a separate file in which i use an axios instance.

   For this,follow the step 3.

3. Create a `js` file(i named it `make_request_to.js`) as follows:

   ```javascript
   import request from '../axios-intances/request'
   export default {
   /* ROUTES */
       get_cases () {
           return request.get('/cases')
       },
       add_new_case (payload) {
           return request.post('/add/case', payload)
       },
   /* OTHER ROUTES ... */
   }
   ```

   

4. The last step is to import the above file in your components :
   `import make_request_to from './make_request_to'`

   And use it like:

   ```javascript
   methods: {
     get_cases () {
       make_request_to.get_cases()
         .then(response => console.log(response))
         .catch(error => console.log(error))
     }
   }
   ```

