# family_library
This is a family library project for N and H.
H is a fresh python developer and this is her first time to develop a full stack SPA(single page application) using Python flask as the backend and Vue.js as the frontend. 

# What are we going to build?
Follow this tutorial (https://testdriven.io/blog/developing-a-single-page-app-with-flask-and-vuejs/) to build a single page app step by step.

We are going to design and build a back-end RESTful API, powered by Python Flask, for a single resource -- books. The API itself should follow RESTful design principles, using the basic HTTP verbs: GET, POST,PUT and DELETE.

We also set up a front-end application with Vue that consumes the back-end API.

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
## The generated project structure
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

## Add a Bootstrap-styled table to the Books component:
- install Bootstrap, a popular CSS framework
```npm install bootstrap@4.3.1. --save```
""""Ignore the warnings for jquery and popper.js. Do NOT add either to your project. More on this later."""
- import the Bootstrap styles to client/src/main.js:
```
import 'bootstrap/dist/css/bootstrap.css';
import Vue from 'vue';
import App from './App.vue';
import router from './router';

Vue.config.productionTip = false;

new Vue({
  router,
  render: h => h(App),
}).$mount('#app');
```
- Add 'bootstrap' to the project's dependencies
```npm i -S bootstrap```
- update Ping.vue to get Bootstrap wired up correctly by using a Button and Container in the Ping.vue template section
```
<template>
  <div class="container">
    <button type="button" class="btn btn-primary">{{ msg }}</button>
  </div>
</template>
```

##Update the server-side app
```
BOOKS = [
    {
        'title': 'On the Road',
        'author': 'Jack Kerouac',
        'read': True
    },
    {
        'title': 'Harry Potter and the Philosopher\'s Stone',
        'author': 'J. K. Rowling',
        'read': False
    },
    {
        'title': 'Green Eggs and Ham',
        'author': 'Dr. Seuss',
        'read': True
    }
]


@app.route('/books', methods=['GET'])
def all_books():
    return jsonify({
        'status': 'success',
        'books': BOOKS
    })
```
### Test on server-side at localhost:5000/books 
The return result in the format of JSON:

```
{
  "books": [
    {
      "author": "Jack Kerouac", 
      "read": true, 
      "title": "On the Road"
    }, 
    {
      "author": "J. K. Rowling", 
      "read": false, 
      "title": "Harry Potter and the Philosopher's Stone"
    }, 
    {
      "author": "Dr. Seuss", 
      "read": true, 
      "title": "Green Eggs and Ham"
    }
  ], 
  "status": "success"
}
```
## Update the client-side component
"""
After the component is initialized, the getBooks() method is called via the created lifecycle hook, which fetches the books from the back-end endpoint we just set up.

Review Instance Lifecycle Hooks for more on the component lifecycle and the available methods.

In the template, we iterated through the list of books via the v-for directive, creating a new table row on each iteration. The index value is used as the key. Finally, v-if is then used to render either Yes or No, indicating whether the user has read the book or not.
"""
###Test from front-end: should read time from Flask now and render on the page
## How to add a new book 
- install [Bootstrap Vue](https://bootstrap-vue.org/) library 
```npm install bootstrap-vue@2.0.0-rc.19. --save```
- Enable the Bootstrap Vue library in client/src/main.js:
```
import 'bootstrap/dist/css/bootstrap.css';
import BootstrapVue from 'bootstrap-vue';
import Vue from 'vue';
import App from './App.vue';
import router from './router';

Vue.use(BootstrapVue);

Vue.config.productionTip = false;

new Vue({
  router,
  render: (h) => h(App),
}).$mount('#app');
```
- update the POST ROUTE from server-side app
```
@app.route('/books',methods=["GET","POST"])
def all_books():
    response_object = {"status":"success"}
    if request.method == "POST":
        post_data = request.get_json()
        BOOKS.append({
            "title": post_data.get("title"),
            "author":post_data.get("author"),
            "read":post_data.get("read")
        })
        response_object["message"] = "Book added!"
    else:
        response_object["books"] = BOOKS
    return jsonify(response_object)
```
- update the client-side app in Books component 
```
<script>
import axios from 'axios';

export default {
  data() {
    return {
      books: [],
      addBookForm: {
        title: '',
        author: '',
        read: [],
      },
    };
  },
  methods: {
    getBooks() {
      const path = 'http://localhost:5000/books';
      axios.get(path)
        .then((res) => {
          this.books = res.data.books;
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.error(error);
        });
    },
    addBook(payload) {
      const path = 'http://localhost:5000/books';
      axios.post(path, payload)
        .then(() => {
          this.getBooks();
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.log(error);
          this.getBooks();
        });
    },
    initForm() {
      this.addBookForm.title = '';
      this.addBookForm.author = '';
      this.addBookForm.read = [];
    },
    onSubmit(evt) {
      evt.preventDefault();
      this.$refs.addBookModal.hide();
      let read = false;
      if (this.addBookForm.read[0]) read = true;
      const payload = {
        title: this.addBookForm.title,
        author: this.addBookForm.author,
        read, // property shorthand
      };
      this.addBook(payload);
      this.initForm();
    },
    onReset(evt) {
      evt.preventDefault();
      this.$refs.addBookModal.hide();
      this.initForm();
    },
  },
  created() {
    this.getBooks();
  },
};
</script>
```
## Update "Add Book" button in the template so that the model is displayed when clicking button
```<button type="button" class="btn btn-success btn-sm" v-b-modal.book-modal>Add Book</button>```

The Books Component should be look like this now:
```
<template>
  <div class="container">
    <div class="row">
      <div class="col-sm-10">
        <h1>Books</h1>
        <hr><br><br>
        <button type="button" class="btn btn-success btn-sm" v-b-modal.book-modal>Add Book</button>
        <br><br>
        <table class="table table-hover">
          <thead>
            <tr>
              <th scope="col">Title</th>
              <th scope="col">Author</th>
              <th scope="col">Read?</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(book, index) in books" :key="index">
              <td>{{ book.title }}</td>
              <td>{{ book.author }}</td>
              <td>
                <span v-if="book.read">Yes</span>
                <span v-else>No</span>
              </td>
              <td>
                <div class="btn-group" role="group">
                  <button type="button" class="btn btn-warning btn-sm">Update</button>
                  <button type="button" class="btn btn-danger btn-sm">Delete</button>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <b-modal ref="addBookModal"
            id="book-modal"
            title="Add a new book"
            hide-footer>
      <b-form @submit="onSubmit" @reset="onReset" class="w-100">
      <b-form-group id="form-title-group"
                    label="Title:"
                    label-for="form-title-input">
          <b-form-input id="form-title-input"
                        type="text"
                        v-model="addBookForm.title"
                        required
                        placeholder="Enter title">
          </b-form-input>
        </b-form-group>
        <b-form-group id="form-author-group"
                      label="Author:"
                      label-for="form-author-input">
            <b-form-input id="form-author-input"
                          type="text"
                          v-model="addBookForm.author"
                          required
                          placeholder="Enter author">
            </b-form-input>
          </b-form-group>
        <b-form-group id="form-read-group">
          <b-form-checkbox-group v-model="addBookForm.read" id="form-checks">
            <b-form-checkbox value="true">Read?</b-form-checkbox>
          </b-form-checkbox-group>
        </b-form-group>
        <b-button-group>
          <b-button type="submit" variant="primary">Submit</b-button>
          <b-button type="reset" variant="danger">Reset</b-button>
        </b-button-group>
      </b-form>
    </b-modal>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      books: [],
      addBookForm: {
        title: '',
        author: '',
        read: [],
      },
    };
  },
  methods: {
    getBooks() {
      const path = 'http://localhost:5000/books';
      axios.get(path)
        .then((res) => {
          this.books = res.data.books;
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.error(error);
        });
    },
    addBook(payload) {
      const path = 'http://localhost:5000/books';
      axios.post(path, payload)
        .then(() => {
          this.getBooks();
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.log(error);
          this.getBooks();
        });
    },
    initForm() {
      this.addBookForm.title = '';
      this.addBookForm.author = '';
      this.addBookForm.read = [];
    },
    onSubmit(evt) {
      evt.preventDefault();
      this.$refs.addBookModal.hide();
      let read = false;
      if (this.addBookForm.read[0]) read = true;
      const payload = {
        title: this.addBookForm.title,
        author: this.addBookForm.author,
        read, // property shorthand
      };
      this.addBook(payload);
      this.initForm();
    },
    onReset(evt) {
      evt.preventDefault();
      this.$refs.addBookModal.hide();
      this.initForm();
    },
  },
  created() {
    this.getBooks();
  },
};
</script>
```
## Add an Alert component to display a message when a book is added
- create a new component called Alert.vue
```
<template>
  <div>
      <b-alert variant="success" show>{{ message }}</b-alert>
      <br>
  </div>
</template>

<script>
export default {
  props: ['message'],
};
</script>
```
- import it into the script section of the Books component and register the component:

import into Books component
```
<script>
import axios from 'axios';
import Alert from './Alert.vue';

...

export default {
  data() {
    return {
      books: [],
      addBookForm: {
        title: '',
        author: '',
        read: [],
      },
    };
  },
  components: {
    alert: Alert,
  },

  ...

};
</script>
 ```
 Reference the new component in the template of Books
```
<template>
  <b-container>
    <b-row>
      <b-col col sm="10">
        <h1>Books</h1>
        <hr><br><br>
        <alert></alert>
        <button type="button" class="btn btn-success btn-sm" v-b-modal.book-modal>Add Book</button>

        ...

      </b-col>
    </b-row>
  </b-container>
</template>
```
- make the alert message dynamic by passing data
in the template alert label
```<alert :message="message"></alert> ```
- add the message to the data options in Books.vue:
```
data() {
  return {
    books: [],
    addBookForm: {
      title: '',
      author: '',
      read: [],
    },
    message: '',
  };
},
```
- update within addBook
```
addBook(payload) {
      const path = 'http://localhost:5000/books';
      axios.post(path, payload)
        .then(() => {
          this.getBooks();
          this.message = 'Book added!';
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.log(error);
          this.getBooks();
        });
    },
  ```
 - add a v-if so the alert is only displayed if showMessage is true
 in template alert label:
 ```<alert message = "message" v-if="showMessage"></alert>```
 - add showMessage to the data
 ```
 export default {
  data() {
    return {
      books: [],
      addBookForm: {
        title: '',
        author: '',
        read: [],
      },
      message: '',
      showMessage: false,
    };
  },
  ```
  - update addBook method
  ```
   addBook(payload) {
      const path = 'http://localhost:5000/books';
      axios.post(path, payload)
        .then(() => {
          this.getBooks();
          this.message = 'Book added!';
          this.showMessage = true;
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.log(error);
          this.getBooks();
        });
    },
  ```


