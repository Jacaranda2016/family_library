# family_library
This is a family library project for N and H.
H is a fresh python developer and this is her first time to develop a full stack SPA(single page application) using Python flask as the backend and Vue.js as the frontend. 


# Install and initical flask app

# Install and config vue/cli
- install vue/cli globally by running this command:
```sudo npm install -g @vue/cli@3.7.0``` 

# vue create client
```
Vue CLI v3.7.0
┌───────────────────────────┐
│  Update available: 4.3.1  │
└───────────────────────────┘
? Please pick a preset: (Use arrow keys)
❯ testdriven.io (babel, eslint) 
  default (babel, eslint) 
  Manually select features
```
- Manually select features and enter
```
Vue CLI v3.7.0
┌───────────────────────────┐
│  Update available: 4.3.1  │
└───────────────────────────┘
? Please pick a preset: Manually select features
? Check the features needed for your project: (Press <space> to select, <a> to toggle all, <i> to invert selection)
❯◉ Babel
 ◯ TypeScript
 ◯ Progressive Web App (PWA) Support
 ◯ Router
 ◯ Vuex
 ◯ CSS Pre-processors
 ◉ Linter / Formatter
 ◯ Unit Testing
 ◯ E2E Testing
```
- press SPACE to select Babel, Router and Linter/Formatter
```
Vue CLI v3.7.0
┌───────────────────────────┐
│  Update available: 4.3.1  │
└───────────────────────────┘
? Please pick a preset: Manually select features
? Check the features needed for your project: Babel, Router, Linter
? Use history mode for router? (Requires proper server setup for index fallback in production) (Y/n)
```
- Select "ESLint + Airbnb config" for the linter 
- Select "Lint on save". 
- Select the "In package.json" option so that configuration is placed in the package.json file instead of in separate configuration files.
- Save this as preset for future projects? n
- Press Enter and start to create.

```
📄  Generating README.md...

🎉  Successfully created project client.
👉  Get started with the following commands:

 $ cd client
 $ npm run serve
```
### The generated project structure
```
% cd client
% ls
README.md		node_modules		package.json		src
babel.config.js		package-lock.json	public
```

## The created index.html file
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>
```
``` <div id="app"></div>```
This is a placeholder that Vue will use to attach the generated HTML and CSS to produce the UI. 

```
├── App.vue
├── assets
│   └── logo.png
├── components
│   └── HelloWorld.vue
├── main.js
├── router
|      |__ index.js
└── views
    ├── About.vue
    └── Home.vue
```
## Run Vue app
```
cd client
npm run serve
```
```
 DONE  Compiled successfully in 2621ms                                                                                                               11:02:41 am


  App running at:
  - Local:   http://localhost:8080/ 
  - Network: http://192.168.0.39:8080/

  Note that the development build is not optimized.
  To create a production build, run npm run build.

```
## To simplify, remove "views" folder
## add Ping.vue within components folder
```
<template>
  <div>
    <p>{{ msg }}</p>
  </div>
</template>

<script>
export default {
  name: 'Ping',
  data() {
    return {
      msg: 'Hello!',
    };
  },
};
</script>

```

## Eidt index.js within router folder
```
import Vue from 'vue';
import Router from 'vue-router';
import Ping from '../components/Ping.vue';

Vue.use(Router);

export default new Router({
  mode: 'history',
  base: process.env.BASE_URL,
  routes: [
    {
      path: '/ping',
      name: 'Ping',
      component: Ping,
    },
  ],
});

```

## Remove the navigation from App.vue
```
<template>
  <div id="app">
    <router-view/>
  </div>
</template>

```
## npm run serve and see "hello" at localhost:8080/ping
## connect the client-side Vue app and back-end Flask app
- install axios libary to connect to send AJAX requests
```
npm install axios@0.18.0  --save
```

## update script section in Ping.vue
before it is like this:
```
<script>
export default {
    name:'Ping',
    data(){
        return{
            msg:'Hello!',
        };
    },
};
</script>
```
Change it to:
```
<script>
import axios from 'axios';

export default {
  name: 'Ping',
  data() {
    return {
      msg: '',
    };
  },
  methods: {
    getMessage() {
      const path = 'http://localhost:5000/ping';
      axios.get(path)
        .then((res) => {
          this.msg = res.data;
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.error(error);
        });
    },
  },
  created() {
    this.getMessage();
  },
};
</script>
```

## Test: run the flask app and we should see "pong!" in the browser
Done.

"""
Essentially, when a response is returned from the back-end, we set msg to the value of data from the response object.

Bootstrap Setup"""

## new components: Books.vue
```
<template>
  <div class = "container">
      <p>books</p>
  </div>
    
</template>
```
## Update the router for this new component
```
import Vue from 'vue';
import Router from 'vue-router';
import Books from '../components/Books.vue';
import Ping from '../components/Ping.vue';

Vue.use(Router);

export default new Router({
  mode: 'history',
  base: process.env.BASE_URL,
  routes: [
    {
      path: '/',
      name: 'Books',
      component: Books,
    },
    {
      path: '/ping',
      name: 'Ping',
      component: Ping,
    },
  ],
});

```



