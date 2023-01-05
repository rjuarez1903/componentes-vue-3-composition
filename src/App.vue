<script setup>
    import { ref, computed, onMounted } from 'vue';

    import PaginatePost from './components/PaginatePost.vue';
    import BlogPost from './components/BlogPost.vue';
    import LoadingSpinner from './components/LoadingSpinner.vue'


    const posts         = ref([])
    const postPorPagina = 10
    const start         = ref(0)
    const end           = ref(postPorPagina)
    const favourite     = ref('')
    const loading       = ref(true)

    const changeFavourite = title => favourite.value = title

    const next = () => {
        start.value += postPorPagina
        end.value += postPorPagina
    }

    const previous = () => {
        start.value -= postPorPagina
        end.value -= postPorPagina
    }
    
    const maxLength = computed(() => posts.value.length)

    const fetchData = async () => {
        try {
            const res   = await fetch('https://jsonplaceholder.typicode.com/posts')
            posts.value = await res.json()
        } catch (error) {
            console.log(error)
        } finally {
            loading.value = false
        }
    }

    onMounted (() => fetchData())

    // fetch('https://jsonplaceholder.typicode.com/posts')
    // .then(response => response.json())
    // .then(data => posts.value = data)
    // .finally(() => loading.value = false)
</script>

<template>

    <LoadingSpinner v-if="loading"/>
    <div class="container" v-else>
        <h1>App</h1>
        <h2>My favourite post: {{ favourite }}</h2>

        <PaginatePost 
            @previous="previous"
            @next="next"
            :start="start"
            :end="end"
            :maxLength="maxLength"
            class="mb-2" 
        />

        <BlogPost 
            v-for="post in posts.slice(start, end)" :key="post.id"
            :title="post.title"
            :id="post.id"
            :body="post.body"
            @changeFavourite="changeFavourite"
            class="mb-2"
        />
    </div>
</template>

<style>

</style>