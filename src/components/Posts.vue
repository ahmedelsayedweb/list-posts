<script setup>
import axios from 'axios';
import { onMounted, ref } from 'vue';
import TableList from './TableList.vue';
// Define reactive variables
const loading = ref(true); // Loading state
const header = ref(['Title', 'User Name', 'User Phone', 'Image']); // Table header
const posts = ref([]); // Array of posts
const users = ref([]); // Array of users
// Fetch data from API on component mount
const fetchData = async () => {
    try {
        const [postsResponse, usersResponse] = await Promise.all([
            axios.get(`${import.meta.env.VITE_BASE_URL}/posts`),
            axios.get(`${import.meta.env.VITE_BASE_URL}/users`)
        ]);
        posts.value = postsResponse.data; // Set posts data
        users.value = usersResponse.data; // Set users data
        loading.value = false; // Set loading state to false
    } catch (error) {
        console.error('Error fetching data:', error);
        loading.value = false; // Set loading state to false in case of error
    }
};
// Fetch data on component mount
onMounted(fetchData);
</script>
<template>
    <div class="relative rounded-xl overflow-auto bg-gray-900 max-w-[93%] m-auto mb-[40px]">
        <div class="shadow-sm overflow-hidden mt-4">
            <div v-if="loading" class="containerLoading">
                <div class="w-12 h-12 rounded-full animate-spin border-4 border-solid border-green-500 border-t-transparent"></div>
            </div>
            <template v-else>
                <template v-if="posts.length">
                    <TableList :header="header" :posts="posts" :users="users" />
                </template>
                <div v-else class="emptyList">
                    <h6>No Show posts</h6>
                </div>
            </template>
        </div>
    </div>
</template>