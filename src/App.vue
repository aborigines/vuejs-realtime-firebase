<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Vuejs 2 & Firebase work shop</h1>
    </div>

    <div class="panel-default">
      <div class="panel-heading">
        <h3>Add Book</h3>
      </div>
      <div class="panel-body">
        <form class="form-inline">
            <div class="form-group">
              <label for="">title</label>
              <input type="text" class="form-control" v-model="newBook.title">
            </div>
            <div class="form-group">
              <label for="">author</label>
              <input type="text" class="form-control" v-model="newBook.author">
            </div>
            <div class="form-group">
              <label for="">url</label>
              <input type="text" class="form-control" v-model="newBook.url">
            </div>
            <div class="form-group">
              <button @click="addBook" type="button" class="btn btn-primary">Add Book</button>
            </div>
        </form>
      </div>
    </div>
    <div class="panel-default">
      <div class="panel-heading">
        <h3>Books lists</h3>
      </div>
      <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>title</th>
              <th>author</th>
              <th>action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="book in books">
              <td><a :href="book.url">{{ book.title }}</a></td>
              <td>{{ book.author }}</td>
              <td><span class="glyphicon glyphicon-trash" @click="removeBook(book)"></span></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import firebase from 'firebase'
import toastr from 'toastr'

let config = {
    apiKey: "AIzaSyAZODdIL7kliMMDrLNca13cQz2AjVtnDr4",
    authDomain: "test-filebase.firebaseapp.com",
    databaseURL: "https://test-filebase.firebaseio.com",
    projectId: "test-filebase",
    storageBucket: "test-filebase.appspot.com",
    messagingSenderId: "12074951987"
}

let app = firebase.initializeApp(config)

let db = app.database()
let booksRef = db.ref('books')

export default {
  name: 'App',
  firebase: {
    books: booksRef
  },
  data() {
    return {
      newBook: {
        title: '',
        author: '',
        url: ''
      }
    }
  },
  methods: {
    addBook() {
      booksRef.push(this.newBook)
      this.newBook.title = ''
      this.newBook.author = ''
      this.newBook.url = ''
    },
    removeBook(book) {
      booksRef.child(book['.key']).remove()
      toastr.success('book removed')
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
