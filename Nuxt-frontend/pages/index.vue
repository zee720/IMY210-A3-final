<script setup>
const config = useRuntimeConfig()

const { data,error } = await useFetch(`${config.public.strapiUrl}/api/posts`, {
    server: false
})

const posts = computed(() => data.value?.data || [])

const selectedCategory = ref("All")

const filteredPosts = computed(() => {
    if(selectedCategory.value === 'All') {
        return posts.value
    }
    return posts.value.filter(post => post.category === selectedCategory.value)
})

</script>

<template>
    <div class="container">
        <h1>Student Life Blog</h1>
        <NuxtLink to="/search" class="nav-link">
            Search Posts
        </NuxtLink>

        <select v-model="selectedCategory" class="filter-dropdown">
            <option value="All">All Categories</option>
            <option value="Campus Life">Campus Life</option>
            <option value="Study Tips">Study Tips</option>
            <option value="Exam Prep">Exam Prep</option>
            <option value="Student Wellness">Student Wellness</option>
            <option value="University News">University News</option>
        </select>

        <div v-if="posts.length === 0">
            No posts found
        </div>

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
    max-width: 1000px;
    margin: auto;
    padding: 40px 20px;
    background: #f8fafc;
    min-height: 100vh;
}

.nav-link {
    display: block;
    text-align: center;
    margin-bottom: 25px;
    color: #2563eb;
    text-decoration: none;
    font-weight: bold;
} 

.filter-dropdown {
    display: block;
    margin: 0 auto 25px;
    padding: 10px;
    border-radius: 8px;
}

h1 {
    text-align: center;
    color: #1e388a;
    margin-bottom: 30px;
}

.post-card {
    border: 1px solid #ddd;
    padding: 20px;
    margin: 20px 0;
    background: white;
    border-radius: 15px;
    box-shadow: 0 4px 12px rgba(0,0,0,0.2);
    transition: transform 0.2s ease;
}

.post-card:hover {
    transform: translateY(-5px);
}

h2 {
    color: #111827;
}

p {
    color: #374151;
    line-height: 1.6;
}
</style>