<template>
    <div>
        <div v-if="isLoading" class="loading">Loading...</div>
        <div v-else>
            <SearchBar v-model:searchQuery="searchQuery"/>
            <div v-if="filteredComments.length > 0">
                <table>
                    <thead>
                        <tr>
                            <th>
                                <div class="header-container">
                                <span>ID</span>
                                <div class="sort-buttons">
                                    <button @click="sortBy('id', 'asc')">&#9650;</button>
                                    <button @click="sortBy('id', 'desc')">&#9660;</button>
                                </div>
                            </div>
                            </th>
                            <th>Name</th>
                            <th>
                                <div class="header-container">
                                <span>Email</span>
                                <div class="sort-buttons">
                                    <button @click="sortBy('email', 'asc')">&#9650;</button>
                                    <button @click="sortBy('email', 'desc')">&#9660;</button>
                                </div>
                            </div>
                            </th>
                            <th>Body</th>
                            <th>Actions</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="comment in paginatedComments" :key="comment.id">
                                <td>{{ comment.id }}</td>  
                                <td v-html="highlightText(comment.name)"></td>
                                <td v-html="highlightText(comment.email)"></td>
                                <td v-html="highlightText(comment.body)"></td>
                            <td>
                                <button class="delete-btn" @click="deleteComment(comment.id)">Delete</button>
                            </td>
                        </tr>
                    </tbody>
                </table>
                <TablePagination
                    :totalItems="filteredComments.length"
                    @pageChange="onPageChange"
                    @itemsPerPageChange="onItemsPerPageChange"
                />
            </div>
            <div v-else class="message">No result found</div>
        </div>
    </div>
</template>

<script>
    import {ref, onMounted, computed} from 'vue';
    import axios from 'axios';
    import SearchBar from './SearchBar.vue';
    import TablePagination from './TablePagination.vue';

    export default {
        name:'DataTable',
        components: {
            SearchBar,
            TablePagination
        },
        props: {
            initialItemsPerPage: {
                type: Number,
                default: 10
            }
        },
        setup(props) {
            const isLoading = ref(true);
            const comments = ref([]);
            const searchQuery = ref('');
            const currentPage = ref(1);
            const itemsPerPage = ref(props.initialItemsPerPage);

            const fetchComments = async() => {
                try {
                    const response = await axios.get('https://jsonplaceholder.typicode.com/comments');
                    comments.value = response.data;
                } catch (e) {
                    console.log(e.message);
                } finally {
                    isLoading.value = false;
                }
            };

            const deleteComment = (id) => {
                comments.value = comments.value.filter(comment => comment.id !== id);
            };

            const highlightText = (text) => {
                if (!searchQuery.value) {
                    return text;
                }
                const query = searchQuery.value.toLowerCase();
                const regex = new RegExp(`(${query})`, 'gi');
                return text.replace(regex, '<span class="highlight">$1</span>');
            };

            const filteredComments = computed(() => {
            if (!searchQuery.value) {
                return comments.value;
            }
            const query = searchQuery.value.toLowerCase();
            return comments.value.filter(comment =>
                comment.name.toLowerCase().includes(query) ||
                comment.email.toLowerCase().includes(query) ||
                comment.body.toLowerCase().includes(query)
            );
            });

            const sortBy = (key, order) => {
                comments.value.sort((a, b) => {
                    if (order === 'asc') {
                        return a[key] > b[key] ? 1 : -1;
                    } else {
                        return a[key] < b[key] ? 1 : -1;
                    }
                });
            };
            
            const paginatedComments = computed(() => {
                if (itemsPerPage.value === 0) {
                    return filteredComments.value;
                } else {
                    const start = (currentPage.value - 1) * itemsPerPage.value;
                    const end = start + itemsPerPage.value;
                    return filteredComments.value.slice(start, end);
                }
            });

            const onPageChange = (page) => {
                currentPage.value = page;
            };

            const onItemsPerPageChange = (count) => {
                itemsPerPage.value = count;
                currentPage.value = 1;
            };

            onMounted(fetchComments);

            return {
                isLoading,
                comments,
                searchQuery,
                highlightText,
                filteredComments,
                paginatedComments,
                deleteComment,
                sortBy,
                onPageChange,
                onItemsPerPageChange
            }
        }
    }

</script>

<style scoped>
.loading {
    text-align: center;
    font-size: 20px;
    margin: 20px 60px;
}

table {
    width: 100%;
    border-collapse: collapse;
}

th, td {
    border: 1px solid #ddd;
    padding: 12px;
    text-align: left;
}

th {
    background-color: #f2f2f2;
}

tr:hover {
    background-color: #f3f2fb;
}

.header-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 20px;
}

.sort-buttons {
    display: flex;
    flex-direction: row;
}

.sort-buttons button {
    background: none;
    border: none;
    cursor: pointer;
    padding: 1px;
}

.delete-btn {
    padding: 8px 12px; 
    color: #db0b20;
    font-weight: bold;
    background-color: #ffffff;
    border: solid 2px #db0b20;
    border-radius: 4px;
    cursor: pointer;
}

.message {
    font-size: 20px;
    color: #333;
    text-align: center;
    padding: 10px;
    background-color: #f7f7f7;
    border: 1px solid #ccc;
    border-radius: 5px;
    margin-top: 10px;
}

</style>
