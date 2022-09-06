<template>
  <div>
    <h2>レシピ</h2>
    <input v-model="book.title" type="text" />
    <input v-model="book.author" type="text" />
    <input v-model="book.foodform" type="text" />
    <input v-model="book.material" type="text" />
    <input v-model="book.category" type="text" />
    <!-- <button @click="onClickEdit">修正</button> -->
    <nuxt-link :to="{ name: 'list' }">
      <p>レシピ一覧へ</p>
    </nuxt-link>
  </div>
</template>


<script lang="ts">
import Vue from 'vue'
import { BookService, BookResponse } from '@/service/book'

type Book = BookResponse

interface DataType {
  book: Book
}

export default Vue.extend({
  async asyncData({ route }) {
    const bookId = Number(route.params.id)
    const book = await BookService.fetchBook(bookId)
    return {
      book,
    }
  },
  data(): DataType {
    return {
      book: {
        id: 0,
        title: '',
        author: '',
        foodform: '',
        material: '',
        category: '',
        
      },
    }
  },
  methods: {
     onClickEdit() {
      const bookId = Number(this.$route.params.id)
      BookService.putBook(bookId, this.book)
      this.$router.push({ name: 'index' })
    }
  }
})
</script>