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

<script>
export default {

}
</script>

<style>

</style>