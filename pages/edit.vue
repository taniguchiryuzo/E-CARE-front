<template>
    <div>
        <ul v-for="(book, i) in books" :key="i">
            <li>{{ book.title }}</li>
            <nuxt-link :to="{ name: 'book-detail-id', params: { id: book.id } }"><button>詳細</button>
            </nuxt-link>
            <button @click="onClickDelete(book.id)">削除</button>
        </ul>
    </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { BookService, BookResponse } from '@/service/book'

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
            form: { title: '', author: '', foodform: '', material: '', category: '' },
            books: [],
        }
    },
    methods: {
        async onClickAdd() {
            await BookService.postBookData({ ...this.form })
            this.books = await BookService.fetchBooks()
            this.form = { title: '', author: '', foodform: '', material: '', category: '' }
        },
        async onClickDelete(bookId: number) {
            await BookService.deleteBook(bookId)
            this.books = await BookService.fetchBooks()
        },
    },
})
</script>
<style>
</style>