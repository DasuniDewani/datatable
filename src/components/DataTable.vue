<template>
    <SearchBar/>
    <table>
        <thead>
            <tr>
                <th>
                    <div class="header-container">
                    <span>ID</span>
                    <div class="sort-buttons">
                        <button>&#9650;</button>
                        <button>&#9660;</button>
                    </div>
                </div>
                </th>
                <th>Name</th>
                <th>
                    <div class="header-container">
                    <span>Email</span>
                    <div class="sort-buttons">
                        <button>&#9650;</button>
                        <button>&#9660;</button>
                    </div>
                </div>
                </th>
                <th>Body</th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="comment in comments" :key="comment.id">
                    <td>{{ comment.id }}</td>  
                    <td>{{ comment.name }}</td>
                    <td>{{ comment.email }}</td>
                    <td>{{ comment.body }}</td>
                <td>
                    <button class="delete-btn">Delete</button>
                </td>
            </tr>
        </tbody>
    </table>
    <TablePagination/>
</template>

<script>
    import {ref, onMounted} from 'vue';
    import axios from 'axios';
    import SearchBar from './SearchBar.vue';
    import TablePagination from './TablePagination.vue';

    export default {
        name:'DataTable',
        components: {
            SearchBar,
            TablePagination
        },
        setup() {
            const comments = ref([]);

            const fetchComments = async() => {
                try {
                    const response = await axios.get('https://jsonplaceholder.typicode.com/comments');
                    comments.value = response.data;
                    console.log(comments.value);
                } catch (e) {
                    console.log(e.message);
                }
            };
            onMounted(fetchComments);

            return {
                comments: comments
            }
        }

    }

</script>

<style scoped>

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

</style>
