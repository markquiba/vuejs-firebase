<template>
  <div id="home" class="container">
    <div class="card">
      <div class="card-body">
        <form id="form" v-on:submit.prevent="addBook">
          <div class="form-row">
            <div class="col">
              <input type="text" class="form-control" placeholder="Title" v-model="newBook.title">
            </div>
            <div class="col">
              <input type="text" class="form-control" placeholder="Author" v-model="newBook.author">
            </div>
            <div class="col">
              <input type="text" class="form-control" placeholder="URL" v-model="newBook.url">
            </div>
            <div class="col">
              <input type="submit" class="form-control btn btn-primary" value="Add Book">
            </div>
          </div>
        </form>
      </div>
    </div>
    <div class="card">
      <div class="card-header">
        <h3>Book List</h3>
      </div>
      <div class="card-body">

        <table class="table">
          <thead class="thead-dark">
            <tr>
              <th scope="col">#</th>
              <th scope="col">Title</th>
              <th scope="col">Author</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(book, index) in books">
              <th scope="row">{{ ++index }}</th>
              <td><a v-bind:href="book.url">{{ book.title }}</a></td>
              <td>{{ book.author }}</td>
              <td><button type="button" class="btn btn-outline-danger" v-on:click="deleteBook(book)"><i class="fa fa-trash-o" aria-hidden="true"></i></button></td>
            </tr>
          </tbody>
        </table>

      </div>
    </div>
  </div>
</template>

<script>

import Firebase from 'firebase'

import toastr from 'toastr'

let config = {
  apiKey: "AIzaSyBH4IDl2-oEQwLA--u_R7hG2oIRHga03PM",
  authDomain: "vuejs-firebase-02-284b3.firebaseapp.com",
  databaseURL: "https://vuejs-firebase-02-284b3.firebaseio.com",
  projectId: "vuejs-firebase-02-284b3",
  storageBucket: "vuejs-firebase-02-284b3.appspot.com",
  messagingSenderId: "724139514246"
}

let app = Firebase.initializeApp(config);
let db = app.database();

let booksRef = db.ref('books');




export default {
  name: 'HomePage',
  firebase: {
    books: booksRef
  },
  data () {
    return {
      newBook: {
        title: '',
        author: '',
        url: ''
      }
    }
  },
  methods: {
    addBook: function() {
      booksRef.push(this.newBook);
      this.newBook.title = '';
      this.newBook.author = '';
      this.newBook.url = '';
      toastr.success('Book added!');
    },
    deleteBook: function(book) {
      booksRef.child(book['.key']).remove();
      toastr.success('Book removed!');
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .card {
    margin: 5vh auto;
  }
</style>
