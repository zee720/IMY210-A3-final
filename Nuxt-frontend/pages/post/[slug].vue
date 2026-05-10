<script setup>
const route = useRoute()
const config = useRuntimeConfig()

const { data } = await useFetch(`${config.public.strapiUrl}/api/posts?filters[slug][$eq]=${route.params.slug}`)

const post = computed(() => data.value?.data?.[0] || null)
</script>


<template>
    <div class="container">
        <div v-if="post">
            <h1>{{ post.title }}</h1>
            <p><strong>Author:</strong> {{ post.author }}</p>

            <div v-for="block in post.content" :key="block">
                <p v-if="block.type === 'paragraph'">
                    {{ block.children[0].text }}
                </p>
            </div>
        </div>
            
        <div v-else>
            Post not found
        </div>
    </div>
</template>

<style scoped>
.container {
    max-width: 900px;
    margin: auto;
    padding: 40px;
}
</style>