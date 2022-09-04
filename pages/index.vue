<template>
  <div>
    <h2>E-CARE</h2>
    <ul v-for="(book, i) in books" :key="i">
      <li>{{ book.title }}</li>
      <nuxt-link :to="{ name: 'book-detail-id', params: { id: book.id } }"><button>詳細</button>
      </nuxt-link>
      <button @click="onClickDelete(book.id)">削除</button>
    </ul>
    <h3>新規追加</h3>
    <input v-model="form.title" type="text" placeholder="料理名" /><br />
    <input v-model="form.author" type="text" placeholder="紹介" /><br />
    <input v-model="form.foodform" type="text" placeholder="食形態" /><br />
    <input v-model="form.material" type="text" placeholder="材料" /><br />
    <input v-model="form.category" type="text" placeholder="カテゴリ" /><br />
    <button @click="onClickAdd">追加</button>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import {BookService, BookResponse } from '@/service/book'

interface Form {
  title: string
  author: string
  foodform: string
  material: string
  category: string
}

type Book = BookResponse

interface DataType {
  form: Form
  books: Book[]
}

export default Vue.extend({
  async asyncData() {
    const books = await BookService.fetchBooks()
    return {
      books,
    }
  },
  data(): DataType {
    return {
      form: { title: '', author: '' ,foodform:'',material:'',category:''},
      books: [],
    }
  },
  methods: {
    async onClickAdd() {
      await BookService.postBookData({ ...this.form })
      this.books = await BookService.fetchBooks()
      this.form = { title: '', author: '' , foodform:'',material:'',category:''}
    },
    async onClickDelete(bookId: number) {
      await BookService.deleteBook(bookId)
      this.books = await BookService.fetchBooks()
    },
  },
})
</script>