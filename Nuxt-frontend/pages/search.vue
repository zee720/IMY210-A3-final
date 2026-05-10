<script setup>
const config = useRuntimeConfig()

const { data } = await useFetch(`${config.public.strapiUrl}/api/posts`)

const posts = computed(() => data.value?.data || [])

const searchQuery = ref('')

const filteredPosts = computed(() => {
  return posts.value.filter(post =>
    post.title?.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
    post.author?.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})

</script>

<template>
  <div class="container">
    <h1>Search Posts</h1>
    <NuxtLink to="/" class="nav-link">
      Back to Home
    </NuxtLink>

    <input v-model="searchQuery" type="text" placeholder="Search by title or author..."
            class="search-box"/>

    <div v-for="post in filteredPosts" :key="post.id" class="post-card">
      <NuxtLink :to="`/post/${post.slug}`">
        <h2>{{ post.title }}</h2>
      </NuxtLink>

      <p><strong>Author:</strong> {{ post.author }}</p>
      <p>{{ post.description }}</p>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 900px;
  margin: auto;
  padding: 40px;
}

.nav-link {
    display: block;
    text-align: center;
    margin-bottom: 25px;
    color: #2563eb;
    text-decoration: none;
    font-weight: bold;
} 

.search-box {
  width: 100%;
  padding: 12px;
  margin-bottom: 20px;
}

.post-card {
  border: 1px solid #ddd;
  padding: 20px;
  margin: 20px 0;
  border-radius: 10px;
}
</style>