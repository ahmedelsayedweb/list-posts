<script setup>
const { users, posts, header } = defineProps({
    users: { type: Array, required: true },
    post: { type: Object, required: true },
})
// Placeholder image function for error handle
const handleErrorImage = (e) => {
    e.target.src = 'https://ralfvanveen.com/wp-content/uploads//2021/06/Placeholder-_-Begrippenlijst.svg'
}
// Get user details by user ID
const getUser = (userId) => {
    return users.find(item => item.id === userId);
}
// Get image URL for post
const getUrlImage = (postId) => {
    return `https://picsum.photos/600/300/?image=${postId}`;
}
</script>
<template>
    <td class="border-b border-slate-100 dark:border-slate-700 p-4 pl-8 text-gray-300 dark:text-gray-300">
        {{ post.title }}
    </td>
    <td class="border-b border-slate-100 dark:border-slate-700 p-4 pl-8 text-gray-300 dark:text-gray-300">
        {{ getUser(post.userId)?.username }}
    </td>
    <td class="border-b border-slate-100 dark:border-slate-700 p-4 pl-8 text-gray-300 dark:text-gray-300">
        {{ getUser(post.userId)?.phone }}
    </td>
    <td class="border-b border-slate-100 dark:border-slate-700 p-4 pl-8 text-gray-300 dark:text-gray-300">
        <img class="image" @error="handleErrorImage" :src="getUrlImage(post.id)" />
    </td>
</template>