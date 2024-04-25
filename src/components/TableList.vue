<script setup>
import { computed, ref } from 'vue';
const { users,posts,header } = defineProps({
    users: { type: Array, required: true },
    posts: { type: Array, required: true },
    header: { type: Array, required: true},
})
const itemsPerPage = ref(10); // Number of items per page
const currentPage = ref(1); // Current page
const totalPages = computed(() => Math.ceil(posts.length / itemsPerPage.value)); // Total pages
const startIndex = computed(() => (currentPage.value - 1) * itemsPerPage.value); // Start index for pagination
const paginatedItems = computed(() => {
    return posts.slice(startIndex.value, startIndex.value + itemsPerPage.value); // Paginated items
});
// Placeholder image function for error handling
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
// Set current page
const setCurrentPage = (numberPage) => {
    currentPage.value = numberPage;
}
// Method to go to the previous page
const previousPage = () => {
    if (currentPage.value > 1) {
        currentPage.value--;
    }
}
// Method to go to the next page
const nextPage = () => {
    if (currentPage.value < totalPages.value) {
        currentPage.value++;
    }
}
</script>

<template>
    <table class="border-collapse table-auto w-full text-sm">
        <thead>
            <tr>
                <th :key="item" v-for="item in header"
                    class="border-b dark:border-slate-600 font-medium p-4 pl-8 pt-0 pb-3 text-gray-300 dark:text-slate-200 text-left">
                    {{ item }}
                </th>
            </tr>
        </thead>
        <tbody class="bg-white dark:bg-slate-800">
            <tr :key="post.id" v-for="post in paginatedItems">
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
            </tr>
        </tbody>
    </table>
    <div class="flex items-center justify-between border-t px-4 py-3 sm:px-6">
        <div class="flex flex-1 justify-between sm:hidden">
            <a class="relative inline-flex items-center rounded-md border border-gray-300 bg-white px-4 py-2 text-sm font-medium text-white hover:bg-gray-50">Previous</a>
            <a class="relative ml-3 inline-flex items-center rounded-md border border-gray-300 bg-white px-4 py-2 text-sm font-medium text-white hover:bg-gray-50">Next</a>
        </div>
        <div class="hidden sm:flex sm:flex-1 sm:items-center sm:justify-between">
            <div>
                <p class="text-sm text-white">
                    Showing
                    <span class="font-medium">{{ startIndex }}</span>
                    to
                    <span class="font-medium">{{ startIndex + itemsPerPage < posts.length ? startIndex + itemsPerPage : 100 }}</span>
                    of
                    <span class="font-medium">{{ posts.length }}</span> results
                </p>
            </div>
            <div>
                <div class="isolate inline-flex -space-x-px rounded-md shadow-sm" aria-label="Pagination">
                    <button @click="previousPage"
                        class="relative inline-flex items-center rounded-l-md px-2 py-2 text-white ring-1 ring-inset ring-gray-300 hover:bg-indigo-600 focus:z-20 focus:outline-offset-0">
                        <span class="sr-only">Previous</span>
                        <svg class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                            <path fill-rule="evenodd" d="M12.79 5.23a.75.75 0 01-.02 1.06L8.832 10l3.938 3.71a.75.75 0 11-1.04 1.08l-4.5-4.25a.75.75 0 010-1.08l4.5-4.25a.75.75 0 011.06.02z" clip-rule="evenodd" />
                        </svg>
                    </button>
                    <button @click="setCurrentPage(item)" :key="item" v-for="item in totalPages"
                        :class="`relative items-center px-4 py-2 text-sm font-semibold text-white ring-1 ring-inset ring-gray-300  focus:z-20 focus:outline-offset-0 md:inline-flex ${item == currentPage ? 'bg-indigo-600 text-white' : 'hover:bg-indigo-600'}`">
                        {{ item }}
                    </button>
                    <button @click="nextPage" class="relative inline-flex items-center rounded-r-md px-2 py-2 text-white ring-1 ring-inset ring-gray-300 hover:bg-indigo-600 focus:z-20 focus:outline-offset-0">
                        <span class="sr-only">Next</span>
                        <svg class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                            <path fill-rule="evenodd" d="M7.21 14.77a.75.75 0 01.02-1.06L11.168 10 7.23 6.29a.75.75 0 111.04-1.08l4.5 4.25a.75.75 0 010 1.08l-4.5 4.25a.75.75 0 01-1.06-.02z" clip-rule="evenodd" />
                        </svg>
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>
